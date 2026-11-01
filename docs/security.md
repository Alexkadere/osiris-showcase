# OSIRIS — Security model

OSIRIS runs on my home server and can see my media, my games, my devices and my disks. The design goal is simple: **the web application should be able to show everything, but do very little on its own.** This page lists the measures that are actually in place, each one checked against the private code, its configuration and its tests.

[← Back to the README](../README.md)

## At a glance

| Area | Measure |
|---|---|
| Network exposure | Listens on `127.0.0.1` only; HTTPS through Tailscale Serve, tailnet only |
| Secrets | Server-only file, never in Git, never in the Docker build context |
| Container | Read-only filesystem, all capabilities dropped, `no-new-privileges`, non-root user |
| Commands | The web application never starts a process; no Docker socket, no sudo |
| Host access | Separate systemd services with their own unprivileged users, exchanging files only |
| Integrations | Read-only use of TMDB, Jellyfin, Steam, IGDB and Tailscale |
| Deletion | Never automatic for user files; always an explicit, confirmed action |
| Data changes | Additive migrations, tried on a copy of production, backup before every update |
| Proof | Isolation tests that fail if these rules are broken |

## 1. Access limited to my tailnet

- The container listens on **`127.0.0.1` only**. No port is published to the local network or the internet.
- **Tailscale Serve** provides HTTPS **inside my tailnet only**; Tailscale Funnel (public exposure) is never used.
- OSIRIS has **no login of its own yet**: access control is membership of my tailnet. Any wider exposure is explicitly conditioned on designing authentication first.
- Endpoints that write files (transfers) only accept requests from OSIRIS's own pages: the browser's `Sec-Fetch-Site` must be `same-origin`, and a custom header is required, which another site cannot add without a CORS preflight that OSIRIS never answers.

## 2. Secrets stay on the server

- API keys and account identifiers (TMDB, Jellyfin, Steam, IGDB) live in an environment file on the server, readable by its owner only.
- That file and any copy of it are **excluded from Git and from the Docker build context**; an automated test fails if an environment file, a database or local server notes would be tracked.
- Keys are used on the server side only and never sent to the browser or written to a page.
- CI builds and tests the application **without any secret**.

## 3. A hardened container

- Read-only root filesystem, temporary files in memory (`tmpfs`).
- **All Linux capabilities dropped**, `no-new-privileges`.
- Runs as the **non-root** `node` user, with a health check.
- **No Docker socket** mounted, so the application cannot control other containers.
- It can write only to its database, its backups, a request folder and the transfer deposit. Everything else it sees (monitoring, storage, devices, Wake-on-LAN status) is mounted **read-only**.

## 4. The application runs no commands

- The application code contains no process creation, no shell and no `sudo`; a test enforces that the web application stays away from Docker and from commands.
- Tasks that need the host are performed by dedicated services (below). OSIRIS can only **ask** for one, by writing a small request file that contains an identifier and a date, nothing that could choose a target or a command.

## 5. Separate, sandboxed host services

Each host service is a short Python script (standard library only) started by systemd with `NoNewPrivileges`, an **empty capability set**, `ProtectSystem=strict`, a system-call filter and a memory limit.

| Service | User | Network |
|---|---|---|
| Monitoring agent | unprivileged, shared with the storage scanner | **no IP network** (Unix sockets only); reads only, with fixed read-only commands and no shell |
| Storage scanner | same | **no network at all**, lowest CPU and I/O priority; opens files read-only and never follows links |
| Device collector | its own | **no network at all**; a single `GET` of the local Tailscale status, never a request that changes Tailscale |
| Wake-on-LAN service | its own | every address denied except the **LAN broadcast address** |

## 6. Wake-on-LAN, isolated by design

- OSIRIS never sends a network packet itself. It writes a request; the dedicated service sends the magic packet.
- The service can wake **one device only**, fixed in a root-only configuration on the server; the MAC address never appears in OSIRIS, its database or its status files.
- If the network allowance is missing, sending fails closed.
- Requests that are already handled, older than 2 minutes, or arrive less than 30 seconds after the last successful send are refused; OSIRIS itself refuses a second request within a minute.
- Every send needs a **confirmation dialog**. Afterwards OSIRIS only reports what it can observe: "signal sent", then "Tailscale connection detected", **never** "powered on".
- OSIRIS cannot shut down or restart the PC. A clean shutdown is planned for the future Windows agent, always behind a confirmation.

## 7. File transfers limited to one folder

- A **single folder** on the server, and a **single module** in the code allowed to touch it (enforced by a test).
- Files are stored under **random identifiers**; the name typed in the browser is cleaned, kept in the database and never used as a path.
- **Quota and disk reserve**: each upload reserves its full announced size under a lock, so concurrent uploads can never exceed the quota or eat into the free-space reserve; free space is re-checked before every chunk.
- Each chunk is accepted only at the expected position and size; a file is offered only after its **SHA-256** has been computed and it has been renamed atomically.
- Downloads are always attachments, served as `application/octet-stream` with `X-Content-Type-Options: nosniff` and a sandboxing `Content-Security-Policy`: OSIRIS never displays an uploaded file.
- The "check the deposit" tool reconciles the database and the folder **without ever deleting** anything.

## 8. Read-only by default

- **Jellyfin, Steam, TMDB and IGDB** are only read. A synchronization records observations and can move a status forward, but **never overwrites** a rating, a favorite, a note, a session or a manual choice; tests install SQLite triggers that make any such write fail.
- **Storage analysis** is strictly read-only: no delete, move or clean-up action exists in the interface.
- **Device inventory** only reads Tailscale's status; nothing is stored except the names and types I choose.
- **Demo mode** uses fictitious data and refuses every write on the server.

## 9. No automatic deletion, confirmations for sensitive actions

- OSIRIS never deletes a user file by itself, and the assisted clean-up feature is on hold on purpose. No AI recommendation can trigger a deletion.
- Removing a title or a game, deleting a session, deleting a transferred file, starting a deep storage analysis and sending a Wake-on-LAN signal all go through an **explicit confirmation**.
- The only automatic pruning is the rotation of OSIRIS's own database backups (14 kept).

## 10. Careful data changes

- Database migrations are **additive only**: existing tables and data are never rewritten or dropped, and a migration is never edited once applied.
- Each migration has a **data-preservation test**, and is tried on a **read-only copy of the production database** before being applied.
- The update script always takes a **verified backup** before migrating; a backup also runs every night.
- Every production action (deployment, migration, new service) needs my explicit approval.

## 11. Enforced by tests

Rules that matter are checked automatically, so a regression fails the build instead of reaching production:

- **Isolation tests** for each sensitive area: only the collector talks to Tailscale, and with one `GET`; OSIRIS has no network API for Wake-on-LAN and writes a single request file; only one module touches the transfer folder; the storage scanner never deletes, moves, renames or runs anything; the agent and its systemd units run without network or privileges; Jellyfin code never writes personal data.
- **Repository hygiene**: no database, environment file or server notes can be tracked.
- **Test isolation**: every test uses temporary databases and fake servers; a guard makes any request to a non-local host fail, and the end-to-end suite fingerprints the local personal database before and after the run.

## Known limits

- No user authentication yet: OSIRIS is meant for a private tailnet only.
- The container uses the host network namespace (while listening on loopback only) so that it can reach Jellyfin and the external APIs.
- Security relies on configuration that lives on the server and is reviewed manually at each deployment.

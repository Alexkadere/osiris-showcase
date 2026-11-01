# OSIRIS — Testing and quality

OSIRIS manages my real data and talks to my real server, so tests have two jobs: prove that features work, and prove that **nothing in a test can reach real data, real keys or the real network**.

[← Back to the README](../README.md)

## In numbers

| Suite | Tool | Count | Result |
|---|---|---:|---|
| Unit and integration | Vitest | **1,316 tests** in 133 files | all passing |
| End-to-end | Playwright (Chromium) | **150 scenarios** in 37 files: 118 desktop, 16 tablet, 16 mobile | all passing (see note) |
| Host services | Python `unittest` | **88 tests** | all passing (5 POSIX-only tests run on Linux CI) |

<sub>Counted and run locally on 1 November 2026, on the code deployed in production that day. Note: in the full Playwright run, one scenario exceeded its time limit on the first pass, then passed three times in a row when run alone; CI retries a failed scenario once.</sub>

## Test levels

### Unit tests

Domain rules in isolation: library statuses and filters, accent-insensitive search, ranking rules, playtime arithmetic, Steam and Jellyfin reconciliation rules, device states, Wake-on-LAN messages, transfer limits and file-name cleaning, `Content-Disposition` and `Range` parsing, pagination, Zod schemas.

### Integration tests

- **Services and Server Actions on temporary SQLite databases**: validation, duplicates, transactions, demo-mode refusals, recomputed totals.
- **Connectors against fake local servers** (TMDB, Jellyfin, Steam, IGDB): success, errors, quotas, private profiles, malformed data.
- **Schedulers** with injected timers, then with the real sync engine against a fake Jellyfin server.
- **Personal-data guard**: during Jellyfin sync tests, SQLite triggers make any write to ratings, notes or favorites fail.
- **File transfers** on a temporary folder: concurrent uploads never exceed the quota or the disk reserve, wrong-position chunks are refused, resume only with the same file, SHA-256 and atomic rename, incomplete files never downloadable, reconciliation never deletes.

### Contract tests

The real Python services (monitoring agent, storage scanner, device collector, Wake-on-LAN service) are run on fake data, and their output files must be **accepted by OSIRIS's own schemas**. A change on one side that breaks the other fails the build.

### Migration tests

Each schema migration has a test that fills the previous schema with data, applies the migration and checks that **every existing table and row is identical** and that new constraints hold. Before reaching production, a migration is also tried on a **read-only copy of the production database**, then compared table by table.

### Isolation and hygiene tests

Static and runtime checks that fail if a safety rule is broken: the web application never imports process or Docker APIs, only one module touches the transfer folder, only the collector talks to Tailscale, the Wake-on-LAN service unit allows no address in the repository, the storage scanner never deletes or moves anything, Jellyfin code never writes personal data, and no database or environment file can be tracked by Git.

### Python tests

Host services are tested on **temporary directory trees and fake sockets**: measurements, hard links counted once, verified duplicates, read budgets and time limits, symbolic links never followed, and a check that an analysis leaves the tree **byte-for-byte unchanged**. The Wake-on-LAN service is tested with a fake socket: nothing is ever sent.

### End-to-end tests

Real user journeys in Chromium against a production build of OSIRIS:

- **Media**: adding by TMDB search, statuses, keyboard rating, favorites, progress, filters, pagination, rankings reordered by keyboard, mouse and touch.
- **Games**: IGDB-only adding, sessions, statistics, Steam sync against fake servers, origin labels, Top 10 Games.
- **Server**: live measurements every second, stale or missing agent, storage analysis and on-demand deep scan.
- **Devices**: the four states, renamed and missing devices, stale readings never shown as current.
- **Wake-on-LAN**: the test plays the server's service; cancel writes nothing, a request writes one file, "Tailscale connection detected" is shown, never "powered on".
- **Transfers**: real files uploaded in chunks, paused, cut, resumed after reload with the same file only, downloaded identical with `Range`, refused past the quota, deleted only after confirmation.
- **Errors**: TMDB failures and quotas, network cuts, a Server Action whose response is lost after the server applied it.

Three viewports: **desktop 1440 × 900** (every journey), **tablet 768 × 1024** and **mobile** (responsive journeys). Responsive tests check that no page scrolls horizontally, that touch targets are at least 44 px, and that iPhone safe areas (emulated through the Chrome DevTools Protocol) keep headers and the tab bar clear of the notch.

## Isolation of the test environment

- Each end-to-end run creates **its own temporary folder** with a fresh, migrated SQLite database, then deletes it.
- **Fake TMDB, Jellyfin, Steam and IGDB servers** run on local ports; the application is pointed at them with fictitious keys.
- **Every variable OSIRIS reads is imposed** when the test server starts, so a developer's local configuration can never leak into a test.
- Before any write, the database path is resolved exactly like the application does and refused if it is not inside the run folder.
- The developer's local database is **fingerprinted** (size, date, SHA-256) before and after the run; any difference fails the suite.
- A **network guard** makes every request to a non-local host fail during unit and integration tests.
- The first end-to-end test proves the isolation at runtime: a write lands in the temporary database, a search reaches the fake TMDB.

## Continuous integration

GitHub Actions runs on every push to `main` and every pull request, with **no database and no secret**:

| Job | Steps |
|---|---|
| **Checks** | ESLint, TypeScript, Prettier, migrations consistent with the schema, Vitest, Python tests, systemd unit syntax, production build |
| **End-to-end** | Production build, then the full Playwright suite; HTML report kept on failure |
| **Docker** | Image build, first installation on a temporary folder, start with the production Compose file (read-only, capabilities dropped), health check, pages, backup and migration |

## Validated on real hardware

Automated tests use fictitious data only. Features that touch the physical world are additionally validated by hand, with me, before they are called done:

- **Wake-on-LAN**: from sleep, then from a **full shutdown** of my desktop PC (after fixing the BIOS power settings it needed).
- **File transfers**: a photo sent from my iPhone, verified by SHA-256 on the server, downloaded on the PC byte-for-byte identical, and a resumed download (`206 Partial Content`).
- **iPhone layout**: checked on the device itself, as a home-screen app.

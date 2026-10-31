<div align="center">

# OSIRIS

### Your world. One system.

**Organization, Supervision & Integration of Resources, Information & Services**

A personal, self-hosted workspace to bring media, server monitoring and future connected devices into one interface.

**Portfolio showcase · Original application and source code are private**

</div>

---

## Overview

OSIRIS is an evolving personal web application that I designed to centralize services and information I use every day. Instead of switching between a media library, system dashboards and project trackers, I want a coherent, modular interface that brings them together.

The project combines my interest in **software development, infrastructure, automation and robotics**. Its interface is influenced by the clarity of Linear, the visual organization of Plex and the dashboards of Homarr, with a distinct graphite-and-steel-blue design.

> **What is this repository?** A public, curated **project showcase**, not the application's source code. Development takes place in a separate private repository. No credentials, infrastructure configuration, private data or production database are shared here.

## Features

| Area | What OSIRIS does | Status |
| --- | --- | --- |
| **Movies, TV & Animation** | Persistent TMDB-powered libraries, including a dedicated animation section | Implemented |
| **Jellyfin** | Read-only scheduled synchronization and a dashboard of recent watches | Implemented |
| **Server dashboard** | Linux CPU, memory, temperatures, disk and service monitoring | Implemented |
| **Storage analysis** | Storage overview and duplicate-file analysis, without automatic deletion | Implemented |
| **Rankings** | Separate Top 10 lists for films, TV series and animation | Implemented |
| **Responsive UI** | Desktop and mobile layouts, including an iPhone-friendly navigation | Implemented |
| **Games library** | Persistent games catalog, ratings, favorites, sessions, played-time statistics and filters | Implemented |
| **Steam & IGDB** | Read-only Steam game, playtime and achievement synchronization; IGDB search and metadata enrichment | Implemented |
| **Projects** | Project pages, progress and task tracking | Planned / prototype UI |
| **Connected devices** | Home Assistant, RGB lighting, 3D printer and robotics integrations | Future exploration |

Statuses reflect the private project's deployment notes through **31 October 2026**. The application is under active development; the showcase does not provide a live public demo.

## Design direction

- **Graphite / steel-blue** color palette, minimal ornamentation and clear information hierarchy.
- A unified interface for media, server and project information.
- Responsive navigation adapted to desktop and mobile devices.
- Explicit distinction between **real data**, **demo content** and **features not yet available**.

### Screenshots

*Curated screenshots of the actual application will be added here after removing personal media, service details, identifiers and network information.*

<!-- Screenshot examples, once safe files exist:
![OSIRIS dashboard](assets/dashboard.png)
![Media library](assets/media-library.png)
![Server monitoring](assets/server-dashboard.png)
-->

## Technology

| Layer | Technologies used in the project |
| --- | --- |
| Web application | Next.js, React, TypeScript |
| Interface | Tailwind CSS, shadcn/ui |
| Database | SQLite, Prisma |
| Validation | Zod |
| Integrations | TMDB, Jellyfin |
| Host monitoring | Python agent on Linux |
| Deployment | Docker Compose, private Tailscale access |
| Quality | GitHub Actions, Vitest, Playwright, ESLint, Prettier |

**A note on skills:** The application has been built with assistance from AI coding tools. The technologies above describe the **project's stack**, not a claim that I independently master every framework. I use the project to learn software design, system integration, testing and deployment.

## Engineering challenges

**Keeping personal information safe.** OSIRIS separates personal library entries from external metadata and Jellyfin / Steam observations. Synchronization is designed not to overwrite personal ratings, favorites, reviews or manual gaming sessions.

**Read-only infrastructure monitoring.** A dedicated Linux agent reports system information without giving the web interface administrator privileges or direct access to Docker's control socket.

**Reliable deployment.** The application uses versioned database migrations and documented backup procedures. The production service is accessible privately, rather than exposed to the public internet.

**Connecting external libraries.** The games module combines a local catalog with optional read-only Steam imports and IGDB enrichment, while distinguishing third-party observations from user-managed records.

**Building incrementally.** New features start as prototypes or clearly identified demo modules. Functionality is described as complete only after it is implemented and tested.

## Roadmap

- [x] Media library and TMDB integration
- [x] Jellyfin synchronization
- [x] Linux monitoring and storage analysis
- [x] Personal rankings
- [x] Persistent games collection and gaming statistics
- [x] Steam integration with automatic read-only synchronization
- [x] IGDB game search and metadata enrichment
- [x] Dedicated animation collection and recent watches dashboard
- [ ] Project tracking
- [ ] More detailed episode tracking and export tools
- [ ] Home automation and personal hardware integrations
- [ ] Robotics integrations, including my [robotic arm](https://github.com/Alexkadere/robotic-arm)

## Documentation

- [Architecture overview](docs/architecture.md) — a high-level description without private deployment details
- [Development roadmap](docs/roadmap.md) — shipped, in progress and exploratory features

## About

I'm **Alexandre Goulhot-Lassalle**, a BTS SIO (SLAM) student with a passion for software, robotics and **aerospace exploration**. I build projects to understand how systems work and to connect computing with real-world applications.

[My GitHub profile](https://github.com/Alexkadere)

---

**Public portfolio only.** No source code or private configuration is distributed through this repository. Screenshots, documents and status descriptions may evolve as OSIRIS develops.

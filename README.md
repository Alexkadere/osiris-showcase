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
| **Movies & TV** | Local collection with TMDB metadata, ratings, favorites, statuses, search and filtering | Implemented |
| **Jellyfin** | Read-only synchronization for media availability and viewing progress | Implemented |
| **Server dashboard** | Linux CPU, memory, temperatures, disk and service monitoring | Implemented |
| **Storage analysis** | Storage overview and duplicate-file analysis, without automatic deletion | Implemented |
| **Rankings** | Personal Top 10 lists for movies, series and anime | Implemented |
| **Responsive UI** | Desktop and mobile layouts, including an iPhone-friendly navigation | Implemented |
| **Games library** | Catalog, progress, ratings and possible Steam integration | Planned / prototype UI |
| **Projects** | Project pages, progress and task tracking | Planned / prototype UI |
| **Connected devices** | Home Assistant, RGB lighting, 3D printer and robotics integrations | Future exploration |

Statuses are based on project documentation as of **October 2026**. The application is under active development; the showcase does not provide a live public demo.

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

**Keeping personal information safe.** OSIRIS separates personal library entries from external metadata and Jellyfin observations. Synchronization must not silently overwrite personal ratings or notes.

**Read-only infrastructure monitoring.** A dedicated Linux agent reports system information without giving the web interface administrator privileges or direct access to Docker's control socket.

**Reliable deployment.** The application uses versioned database migrations and documented backup procedures. The production service is accessible privately, rather than exposed to the public internet.

**Building incrementally.** New features start as prototypes or clearly identified demo modules. Functionality is described as complete only after it is implemented and tested.

## Roadmap

- [x] Media library and TMDB integration
- [x] Jellyfin synchronization
- [x] Linux monitoring and storage analysis
- [x] Personal rankings
- [ ] Games collection
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

# OSIRIS — Public architecture overview

This is a **high-level overview** for a public portfolio. It deliberately omits private network information, deployment credentials and configuration.

## Main layers

```text
Responsive Web UI (Next.js / React)
          |
Server-side actions and validation
          |
Domain services
   |         |          |
SQLite     TMDB      Jellyfin (read-only)
   |
Local personal catalog

Separate read-only host-monitoring agent (Python)
          |
System metrics and storage analysis
```

## Principles

**Modularity.** Media, infrastructure and future game/project modules are organized as independent areas while sharing navigation and interface components.

**Persistence.** The media catalog uses SQLite through Prisma. Personal data is conceptually distinct from fetched metadata and from Jellyfin observations.

**Read-only connections.** Monitoring and storage analysis do not require the application to run destructive host commands. Deletion is not part of the current automatic analysis process.

**Private deployment.** The working application is self-hosted on Linux using container orchestration, with access restricted to a private network. This repository does not publish the deployment configuration.

**Testing.** The development project includes unit and browser-based tests and continuous integration checks.

## Technology summary

Next.js / React / TypeScript; Tailwind CSS and shadcn/ui; Prisma and SQLite; Python monitoring components; TMDB and Jellyfin integrations.

## Scope and limitations

This document describes the software at an architectural level, not a reproducible deployment specification. It does not contain source code, database exports, secrets or operational details of my personal infrastructure.

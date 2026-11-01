# OSIRIS — Public roadmap

A summary of what is shipped and where the project is heading. Order and scope may change.

[← Back to the README](../README.md)

## Shipped and in daily use

- [x] Media catalog for films, series and animation, with TMDB metadata and personal data kept separate
- [x] Search, filters, sorting and server-side pagination across all libraries
- [x] Read-only Jellyfin synchronization, scheduled, with "recently watched" on the home page
- [x] Top 10 rankings for films, series, animation and games
- [x] Games library with sessions, playtime, statistics and a mobile command bar
- [x] Read-only Steam synchronization (games, playtime, achievements) and IGDB metadata
- [x] Live server monitoring and read-only storage analysis by separate host services
- [x] Device inventory from Tailscale, with honest connection states
- [x] Wake-on-LAN of my desktop PC, validated from a full shutdown
- [x] File transfers between devices, validated from iPhone to PC
- [x] iPhone home-screen app with safe areas and touch-sized controls
- [x] Automated tests, CI, Docker deployment, backups and documented procedures

## Next major step: Windows agent

A small agent on the desktop PC, so that OSIRIS can show and act on more than its network presence:

- [ ] **Hardware information** on the PC's device page (processor, graphics, memory, storage, temperatures)
- [ ] **Clean shutdown** from OSIRIS, always behind a confirmation
- [ ] **Local RGB lighting control**, handled on the PC itself and never exposed on the network: an on/off switch first, then colors, brightness, effects and profiles

## Later

- [ ] Interface refinements for file transfers (usage bar, sorting and filters, multi-select, notifications)
- [ ] Direct file sending to the PC through the Windows agent
- [ ] Projects module (pages, tasks, documentation)
- [ ] Episode-by-episode tracking for series
- [ ] Monitoring alerts
- [ ] Home automation (scenes, infrared devices)
- [ ] 3D printer status and progress
- [ ] Integration with my robotic arm

## On hold

- Assisted clean-up of files and download automation: paused on purpose. Any future version will require an explicit confirmation per file.

*Status: 1 November 2026.*

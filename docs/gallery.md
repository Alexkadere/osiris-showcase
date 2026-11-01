# OSIRIS — Gallery

Every screenshot was taken on **1 November 2026** from the real application, running locally and isolated from production:

- **demo mode** (marked "Données de démonstration" in the interface), or
- a **local instance with a temporary database** and **fictitious** titles, games, devices and files.

No screenshot comes from the production server or contains a real address, device name, library title or personal file. The interface is in French; desktop shots are 1440 × 900, iPhone shots 393 × 852 (rendered at 2×).

[← Back to the README](../README.md)

## Contents

[Home](#home) · [Media library](#media-library) · [Rankings](#rankings) · [Games](#games) · [Server](#server) · [Devices](#devices) · [Wake-on-LAN](#wake-on-lan) · [Transfers](#transfers)

## Home

<img src="screenshots/01-home-desktop.png" alt="Home page on desktop" width="100%" />

**01 · Home.** Server health at a glance (power, storage, CPU, memory, temperature), recently watched titles and active projects. *Demo mode.*

<img src="screenshots/02-home-iphone.png" alt="Home page on iPhone" width="320" />

**02 · Home on iPhone.** Same information in a single column, with the bottom tab bar. *Demo mode.*

## Media library

<img src="screenshots/03-films-library.png" alt="Films library" width="100%" />

**03 · Films library.** Poster grid with status, favorite, Jellyfin availability and remaining time; search, filters, sorting and server-side pagination. *Demo mode, generated artwork.*

<img src="screenshots/04-film-detail.png" alt="Film detail page" width="100%" />

**04 · Film detail.** Status, half-point rating, favorite, synopsis, director and cast; below, personal notes and editable information. *Isolated instance, fictitious film and people.*

## Rankings

<img src="screenshots/05-top10-films.png" alt="Top 10 Films" width="100%" />

**05 · Top 10 Films.** A podium followed by places 4 to 10; reordered with the mouse, keyboard or touch, and saved only when confirmed. Films, series and animation rankings are mutually exclusive. *Demo mode.*

<img src="screenshots/09-top10-games.png" alt="Top 10 Games" width="100%" />

**09 · Top 10 Games.** Chosen by hand: neither rating nor playtime decides the order. *Demo mode.*

## Games

<img src="screenshots/06-games-library.png" alt="Games library" width="100%" />

**06 · Games library.** Steam and personal games labelled by origin, with progress, playtime and rating; search, "Filtres et tri" and counters. *Demo mode.*

<img src="screenshots/07-game-detail.png" alt="Game detail page" width="100%" />

**07 · Game detail.** Steam playtime counter, sessions, last activity, progress, rating and favorite; further down, the review, the session log and Steam achievements (3 of 5 unlocked). *Isolated instance, fictitious game linked to a fictitious Steam entry; no request to Steam.*

<img src="screenshots/08-game-statistics.png" alt="Game statistics" width="100%" />

**08 · Statistics.** Status breakdown, recent activity by week, platforms and genres, all computed from the library and sessions. *Demo mode.*

<table>
<tr>
<td width="50%"><img src="screenshots/10-games-iphone.png" alt="Games library on iPhone" /></td>
<td width="50%"><img src="screenshots/11-games-filters-iphone.png" alt="Filters bottom sheet on iPhone" /></td>
</tr>
</table>

**10–11 · Games on iPhone.** One-line command bar (search, filters, add) and a bottom sheet whose choices apply immediately. *Demo mode.*

## Server

<img src="screenshots/12-server-overview.png" alt="Server overview" width="100%" />

**12 · Server overview.** Live measurements from the host agent, CPU and memory history, and service status. *Demo mode: the host, services and values are fictitious.*

<img src="screenshots/13-storage-analysis.png" alt="Storage analysis" width="100%" />

**13 · Storage analysis.** Space by category, daily quick analysis (metadata only) and an on-demand deep analysis that confirms duplicates by SHA-256. Read-only: no delete, move or clean-up action exists. *Demo mode.*

## Devices

<img src="screenshots/14-devices.png" alt="Device inventory" width="100%" />

**14 · Device inventory.** Devices of the tailnet as seen by the server, with four distinct states: *Connected to Tailscale*, *Offline · seen 3 days ago*, *Unknown · missing from the last reading*, and *Agent up to date* for the server itself. *Isolated instance, fictitious devices without any address.*

<table>
<tr>
<td width="50%"><img src="screenshots/15-devices-iphone.png" alt="Device inventory on iPhone" /></td>
<td width="50%">

**15 · Devices on iPhone.** Compact rows with large touch targets; the list refreshes itself every 30 seconds while visible.

</td>
</tr>
</table>

<img src="screenshots/16-device-detail.png" alt="Server device page" width="100%" />

**16 · Device page.** Only the sections that have data: Tailscale network, system, and hardware measured by the host agent. *Isolated instance, fictitious server.*

## Wake-on-LAN

<img src="screenshots/17-wake-on-lan-confirm.png" alt="Wake-on-LAN confirmation dialog" width="100%" />

**17 · Confirmation.** Nothing is sent without this dialog, which explains exactly what OSIRIS will and will not be able to see. *Isolated instance: the "signal" only writes a request file in a temporary folder; no packet is sent.*

<img src="screenshots/18-wake-on-lan-detected.png" alt="Tailscale connection detected after the signal" width="100%" />

**18 · Result.** After the server's service reports the signal as sent, OSIRIS waits for the PC to reappear on Tailscale and says so, without claiming that Windows has finished booting. *Isolated instance; the real feature was validated from a full shutdown.*

## Transfers

<img src="screenshots/19-transfers-desktop.png" alt="Transfers on desktop" width="100%" />

**19 · Transfers.** Quota and disk reserve on one line, drop zone, an upload in progress (speed, time left, pause), verified files ready to download or delete, and a deposit check. *Isolated instance: fictitious disk values, files of random bytes with neutral names.*

<img src="screenshots/20-transfers-iphone.png" alt="Transfers on iPhone" width="320" />

**20 · Transfers on iPhone.** A paused upload can be resumed only with the same file (name, size and date checked by the browser and the server). *Isolated instance.*

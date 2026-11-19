<h1 align="center">🏠 Homelab</h1>
<p align="center"><b>A self-hosted home server running 30+ services with Docker Compose.</b><br/>Sanitized public copy: no domains, IPs, credentials or personal data.</p>
<p align="center">
  <img src="https://img.shields.io/badge/Docker_Compose-2496ED?style=flat-square&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Debian-A81D33?style=flat-square&logo=debian&logoColor=white"/>
  <img src="https://img.shields.io/badge/Caddy-1F88C0?style=flat-square&logo=caddy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Cloudflare_Tunnel-F38020?style=flat-square&logo=cloudflare&logoColor=white"/>
  <img src="https://img.shields.io/badge/Immich-4250AF?style=flat-square"/>
  <img src="https://img.shields.io/badge/Jellyfin-00A4DC?style=flat-square&logo=jellyfin&logoColor=white"/>
</p>

## Architecture

```
Internet ──▶ Cloudflare Tunnel ──▶ Caddy (reverse proxy) ──▶ service containers
LAN / Tailscale ───────────────────▲
```

Each service lives in its own folder under [`stacks/`](stacks) with a single `compose.yml`.
Secrets are never in the files: every credential is a `${VARIABLE}` read from an `.env` file kept outside the repo.

## Services

| Area | Services |
|---|---|
| **Media** | Jellyfin, Jellyseerr (Seerr), Radarr, Sonarr, Prowlarr, Bazarr, qBittorrent, Recyclarr, Maintainerr |
| **Music and books** | Navidrome, slskd, Multi-Scrobbler, Audiobookshelf, Kavita, Shelfmark, Chaptarr |
| **Photos and files** | Immich, Stirling-PDF, ConvertX, MeTube, PairDrop |
| **Home** | Home Assistant, Homebox, Mealie, Wallos, OpenGym |
| **Network** | Caddy, AdGuard Home, Cloudflare Tunnel, WatchYourLAN, Speedtest Tracker |
| **Monitoring** | Uptime Kuma, Beszel, Dozzle, Diun, Homepage dashboard, ChangeDetection.io |
| **AI** | Ollama, Open WebUI |
| **Agents** | RSC Brain (built from [ericrisco/rsc-brain](https://github.com/ericrisco/rsc-brain)) |

## Design principles

- **Pinned versions.** Images use fixed tags, many with digests, and an update watcher (Diun) tells me when a new one is out.
- **Secrets stay out of Git.** Compose files only reference environment variables.
- **Least privilege.** The Docker socket is never handed to a container directly: monitoring uses a read-only socket proxy.
- **Health checks everywhere**, so failures show up on the dashboard before I notice them.
- **Backups with a restore test**, not just backups.

## Using a stack

```bash
cd stacks/uptime-kuma
cp .env.example .env   # if the stack has one, then fill in your own values
docker compose up -d
```

Replace `example.com` and the `192.168.1.x` addresses with your own.

## Notes

This is a personal setup shared as a reference. Adjust paths, domains and addresses to your own environment.

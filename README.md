<p align="center">
  <img src="images/JFTD-Logo.png" alt="Joint Task Force Draugr Logo" width="200" />
</p>

# Joint Task Force Draugr — Fluxer Bots

Bot deployment stack for Joint Task Force Draugr (JFTD), an Arma 3 MilSim community on Fluxer.

This repo is a submodule of [Bragi](https://github.com/the-alphabet-cartel/bragi) — the top-level orchestration hub. It manages JFTD's bot stack and references each bot as its own Git submodule.

---

## The Bot Family

| Bot | Role | Description |
|-----|------|-------------|
| [**Ratatoskr**](https://github.com/the-alphabet-cartel/ratatoskr) | Event Scheduler | Manages operation scheduling and attendance tracking. Named for the Norse squirrel who carried messages between the roots and branches of Yggdrasil. |

All bots follow the architectural patterns defined in the [Bragi Clean Architecture Charter](https://github.com/the-alphabet-cartel/bragi/blob/main/docs/standards/charter.md).

---

## Repository Structure

```
jftd/
├── ratatoskr/                ← Git submodule → the-alphabet-cartel/ratatoskr
├── docker-compose.yml        ← JFTD bot stack
├── .env.template             ← Environment variable reference
└── secrets/
    └── README.md             ← Credential setup guide (committed)
```

---

## Deployment

### Prerequisites

- **Server:** Debian Linux with Docker Engine 29.x + Compose v5
- Fluxer bot token for Ratatoskr (see [`secrets/README.md`](secrets/README.md))

### Setup

```bash
# 1. Clone with submodules (from within the bragi repo)
git submodule update --init --recursive

# 2. Create the Docker network
docker network create jftd-net

# 3. Create host directories for persistent data
mkdir -p /opt/bragi/jftd/ratatoskr/{logs,data}

# 4. Configure environment
cp .env.template .env
# Edit .env — set GUILD_ID and bot-specific channel/role IDs

# 5. Create secrets (see secrets/README.md for detailed instructions)

# 6. Deploy the JFTD bot stack
docker compose up -d
```

### Managing the Stack

```bash
# View all bot logs
docker compose logs -f

# Restart a single bot
docker compose restart ratatoskr

# Pull latest images and redeploy
docker compose pull && docker compose up -d
```

---

## Platform

JFTD's bots run on [Fluxer](https://fluxer.gg), a Discord-alternative platform, using the [fluxer-py](https://github.com/akarealemil/fluxer.py) library.

---

*For the fallen and the worthy* ⚔️

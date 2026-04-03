# 🛒 Online Store Pick Simulator

A faithful browser-based recreation of a major UK supermarket's online grocery picking system, built for fun. Simulates the Zebra TC57 handset experience used by in-store pickers fulfilling online orders.

**[Try it](https://insanecipher.github.io/online-store-pick-sim/)**

---

## What is this?

When a customer places an online grocery order, a picker walks the store with a Zebra handheld device, collecting items aisle by aisle and placing them into totes on a trolley. This project recreates that entire flow in the browser — handset UI and all.

---

## Features

- **Faithful handset UI** — recreates the Zebra TC57 screen with real picking flow
- **Serpentine aisle routing** — items are ordered using the same boustrophedon walk real pickers use (up odd aisles, down even aisles)
- **Tote packing** — items are assigned to totes (B1–B4, F1–F4) based on size
- **Quantity confirmation** — items ordered in quantity > 1 trigger a confirm screen
- **Wrong item/tote detection** — error feedback on incorrect scans
- **Hamburger menu** — mirrors the real handset options menu
- **Live item database** — powered by a real PostgreSQL database and FastAPI backend

## Screens

| Screen | Description |
|--------|-------------|
| `index.html` | Pre-shop summary screen (items, IPH stats, trolley type) |
| `picksim-main.html` | Main picking screen — find and scan the correct item |
| `picksim-quantity.html` | Quantity confirmation for multi-quantity items |
| `picksim-tote.html` | Tote placement and scan confirmation |

## Tech Stack

**Frontend**
- Plain HTML, CSS, JavaScript
- Hosted on GitHub Pages

**Backend** (self-hosted on a DreamQuest Mini PC)
- FastAPI (Python)
- PostgreSQL
- Cloudflare Tunnel → `shop.hyperionserver.uk`
- Ubuntu Server 24.04 on Tailscale

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /health` | API and database status |
| `GET /item/{sku}` | Fetch a single item by SKU |
| `GET /items` | List items with optional filters |
| `GET /generate-shop` | Generate a randomised pick list fitted into 8 totes |

## How it works

1. The frontend calls `/generate-shop` to get a randomised ambient pick list
2. Items are sorted into a serpentine aisle walk order
3. The picker is shown the correct item hidden among 7 distractors from the same aisle
4. Clicking the correct item advances to quantity confirmation (if qty > 1) or tote placement
5. The correct tote must be selected from the 8 totes on the trolley
6. Progress is saved in `sessionStorage` across page navigations
7. Session completes when all items are picked

---

*Inspired by real UK supermarket picking systems. Built for fun as a personal learning project. Not affiliated with or endorsed by any retailer.*

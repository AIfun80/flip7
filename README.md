# Flip 7 Scorekeeper

A modern, mobile-friendly real-time scoring companion for the **Flip 7** card game.

- **Live Web App:** [https://aifun80.github.io/flip7/](https://aifun80.github.io/flip7/)
- **GitHub Repository:** [https://github.com/AIfun80/flip7](https://github.com/AIfun80/flip7)

---

## Features

- **Real-Time Multiplayer Sync:** Uses Firebase Realtime Database with anonymous authentication so multiple phones can share, view, and update a game room simultaneously.
- **Instant QR Code Sharing:** Room codes (e.g. `F7_K4M9P2X7Q1`) can be shared via QR code or direct link with automatic clipboard copy.
- **Game Modes:** Supports both **Standard** rules (+2 to +10, x2, +15 Flip 7 Bonus) and **Vengeance** rules (Lucky 13, /2, -2 to -10, Brutal Mode).
- **Interactive Scoring Calculator:** Single-tap number selection, modifier stacking, automatic Flip 7 detection, one-tap Bust button, and manual score entry.
- **Seating & Dealer Order:** Drag-and-drop player rows to match physical table seating with automatic dealer button rotation.
- **Built-in Interactive Guide:** In-app visual walkthrough demonstrating room sharing, game mode toggling, modifier stacking, bust logging, and seating management.
- **Weekly Automated Maintenance:** GitHub Actions workflow scheduled weekly to safely clean up stale, inactive game rooms.

---

## Project Structure

```text
flip7/
├── index.html                   # Complete single-file responsive web app
├── README.md                    # Project documentation
└── .github/
    └── workflows/
        └── cleanup-rooms.yml    # Weekly GitHub Actions room cleanup
```

---

## Local Development & Testing

Open `index.html` directly in any modern browser, or serve locally with any static web server (such as Python, Node.js `http-server`, or the VS Code Live Server extension).

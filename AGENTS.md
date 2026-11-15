# Flip 7 Scorekeeper — Agent Context & Guidelines

This document provides project context, architecture details, security constraints, and operational guidelines for AI agents working in this repository.

---

## 1. Project Overview & Repositories

- **Project Name:** Flip 7 Scorekeeper
- **Live Deployment:** [https://aifun80.github.io/flip7/](https://aifun80.github.io/flip7/)
- **GitHub Repository:** [https://github.com/AIfun80/flip7](https://github.com/AIfun80/flip7)
- **Local Workspace Path:** `C:\Users\chris\.gemini\antigravity\scratch\flip7`
- **Application Type:** Single-file responsive client-side web application (`index.html`).

---

## 2. Architecture & Tech Stack

- **Core File:** `index.html` contains all markup, CSS styles, and JavaScript logic.
- **Styling:** Tailwind CSS (via CDN) with customized mobile-first responsive classes.
- **Icons & QR:** Lucide icons and QRCode.js (via CDN).
- **Zoom Modes:** Three specialized accessibility zoom presets:
  - Default / Standard zoom
  - `she-old` (Comfortable enlarged UI)
  - `even-older` (Large text and buttons)
  - `old-cunt` (Extra-large high-contrast touch targets)
- **Content Security Policy (CSP):** Configured in `<meta http-equiv="Content-Security-Policy">` with `no-referrer`, strict script/style allowances, and HTTPS endpoints.

---

## 3. Realtime Backend & Security

### Firebase Configuration
- **Database URL:** `https://flip7app-dbdfe-default-rtdb.firebaseio.com`
- **Authentication:** Firebase Anonymous Authentication.
  - Automatically requests anonymous token and refreshes sessions via `https://securetoken.googleapis.com/v1/token?key=...`.
  - Persists session tokens in `localStorage` under `flip7_firebase_auth_v1` to prevent generating redundant user IDs.
  - Sync requests append `?auth=<idToken>` to all RTDB REST endpoints.

### Google Cloud API Key & Security Restrictions
- **Key Location:** `FIREBASE_WEB_API_KEY` in `index.html`.
- **Public Key Status:** This is a public client-side key for browser authentication (GitHub Secret Scanning alert #1 is expected).
- **Google Cloud Console Enforcement:**
  1. **HTTP Referrer Restrictions:** Restricted **strictly** to `https://aifun80.github.io/*` (temporary localhost entries must remain removed in production).
  2. **API Restrictions:** Restricted **only** to **Identity Toolkit API** and **Token Service API**.
- **Database Rules:** Enforced server-side in Firebase Realtime Database Security Rules to restrict room path read/writes to authorized payloads and prevent room flooding.

---

## 4. Game Rules & Features

- **Game Modes:**
  - **Standard Mode:** Number cards 0–12, standard modifiers (+2 through +10, x2 multiplier), and automatic Flip 7 Bonus (+15 points when 7 unique number cards are drawn).
  - **Vengeance Mode:** Adds Lucky 13, /2 halving modifier, negative modifiers (-2 through -10), and Brutal Mode.
- **Scoring Interface:**
  - Multi-modifier stack support.
  - Dark emerald unselected/selected styling for `x2` (`bg-emerald-950/75 border-emerald-500/60 text-emerald-300`).
  - One-tap `💥 Bust (0)` button (`bg-red-950/80 border-red-700 text-red-200`) alongside `Add to Score`.
  - Manual Score tab for typing custom round sums directly.
- **Seating & Table Order:**
  - Drag-and-drop player rows with boxed grip handles (`⋮⋮`).
  - Rotating Dealer badge following the physical seating order after each round.

---

## 5. In-App Interactive Guide

The interactive onboarding guide is driven by `guideSteps` inside `index.html` (approx. line 2413). Any changes to live calculator UI must be matched in the Guide mockups:
- **Step 1:** Share Room & Add to Score (shows 10-char room sample `F7_K4M9P2X7Q1`, QR Share icon, Add to Score button).
- **Step 2:** Standard vs. Vengeance mode toggle.
- **Step 3:** Modifier Stacking (`☐ x2` dark emerald badge, number modifiers, Flip 7 bonus).
- **Step 4:** One-Tap Bust Button (`💥 Bust (0)` styling).
- **Step 5:** Seating reordering, Dealer button rotation, and Manual Score tab.

---

## 6. Automated Maintenance

- **Workflow:** `.github/workflows/cleanup-rooms.yml` runs every Sunday at midnight UTC.
- **Function:** Queries Firebase RTDB via GitHub Secrets (`FIREBASE_CLEANUP_TOKEN` or REST auth) to prune game rooms inactive for more than 7 days.

---

## 7. Development & Verification Guidelines for Agents

1. **Keep App Single-File:** Do not split `index.html` into multiple bundles unless explicitly requested by the user.
2. **Syntax Validation:** Always verify `<script>` syntax before finalizing edits:
   ```bash
   node -e "const fs=require('fs');const html=fs.readFileSync('index.html','utf8');const m=html.match(/<script>([\s\S]*?)<\/script>\s*<\/body>/);new Function(m[1]);console.log('SYNTAX_OK');"
   ```
3. **Visual Verification:** Check responsiveness at mobile dimensions (e.g. `360x740`) and across all 3 zoom levels (`she-old`, `even-older`, `old-cunt`) to ensure zero horizontal clipping or button wrapping issues.
4. **Git & GitHub Workflow:**
   - Always ask the user for confirmation before pushing changes to the remote GitHub repository.
   - Remote pushes use GitHub CLI (`gh api -X PUT repos/AIfun80/flip7/contents/<path>`).

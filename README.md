# Casa de Apostas

Internal betting poll for friends. Single-page app, no build step, neon casino vibe.

Live: https://marceloptemization.github.io/casa-de-apostas/

## The game

Each round, the question is: vai aparecer ou não vai aparecer? Friends bet **FALTA** or **VEM**. A bet placed today is for tomorrow's event — the next day, once you know what happened, an admin marks the result and the app celebrates whoever called it right.

## UI

- **Casa tab**: live odds bar, your name + bet buttons (with a reason field that pops out when you pick FALTA), and the outcome card once the result is set.
- **Apostas tab**: list of everyone who bet, their pick (FALTA/VEM), and their reason if they left one.
- **Title and stamp**: framed by Vegas marquee bulbs, with `AMANHÃ` stamped in the corner.
- **Live activity**: a chip slides in from the top-right whenever a new bet comes through polling, with a soft chime.
- **Reveal**: when an admin marks the result, the outcome card spins through `FALTOU? / VEIO?` for ~1.7s before settling. Anyone whose pick matched gets a fullscreen `VENCESTE!` overlay, neon confetti, and a fanfare. Wrong calls get a short sad tone.
- **Sound**: all cues are generated with the Web Audio API (no asset files). First click on the page unlocks the audio context.

## Round lifecycle

`state.date` stores the event date.

- New round → `state.date = tomorrow`. Bets accumulate.
- On the event day → `state.date == today`. Bets stay visible; admin can mark `FALTOU` or `VEIO`.
- The day after the event → `state.date < today` → next client to fetch or poll wipes state and starts a fresh round for tomorrow.

This means rounds last two calendar days (betting day + event day) and the day after wraps back into a new betting day.

Admin can also force a reset mid-round via the `→ começar ronda nova` link in the Apostas tab.

## Admin mode

Admin features (FALTOU/VEIO buttons, undo result, force reset) only show when the URL contains `?admin=on`. There is no localStorage persistence — strip the query string and the controls disappear, so sharing the bare URL with friends never exposes admin actions.

- Enable on your device: `?admin=on`
- Disable (or just open without the param): `?admin=off`

## How it stays in sync

- Shared state (bets, outcome, event date) lives in a [JSONBin](https://jsonbin.io) bin.
- Every open page polls every 5 seconds and pushes after each action.
- Your nickname is cached in `localStorage` so each browser remembers who's betting.

## Deploy

Hosted on GitHub Pages from `main` / root. Push to `main` and the workflow rebuilds within a minute.

## Config

The JSONBin bin ID and master key are hardcoded at the top of `index.html` inside the `CONFIGURAÇÃO` block. If you fork, swap them for your own bin and master key — note that the master key is shipped to the client and visible in browser source, which is the trade-off for having no backend.

## Stack

- Vanilla HTML + CSS + JS, no framework, no build.
- Web Audio API for sound.
- Canvas 2D for confetti.
- JSONBin for shared state.

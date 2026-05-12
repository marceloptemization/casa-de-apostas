# Casa de Apostas

Daily internal betting poll for friends. Single-page app, no build step.

## What it does

Every day, a question is open: vai aparecer ou não vai aparecer? Friends place a bet (FALTA / VEM), and once the day plays out, someone marks the official outcome. The pot resets automatically at midnight.

## How it works

- Single `index.html`, vanilla JS, no framework.
- Shared state (bets, outcome, subject name) lives in a [JSONBin](https://jsonbin.io) bin and is polled every 5 seconds.
- Daily reset is automatic: when the bin's `date` field doesn't match today, the state wipes on the next sync.
- Personal nickname is cached in `localStorage` so each browser remembers who's betting.

## Deploy

Hosted via GitHub Pages from the `main` branch root. Push to `main` and GitHub serves `index.html` at `https://<username>.github.io/<repo-name>/`.

## Config

The JSONBin bin ID and master key are hardcoded at the top of `index.html`. Replace them if forking.

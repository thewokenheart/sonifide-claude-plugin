---
name: status
description: Show what is still unregistered across the user's Sonifide catalog — which tracks are missing a PRO, MLC, SoundExchange or Copyright Office filing, and what is blocking each one. Use when the user runs /sonifide:status or asks what still needs to be registered or filed.
---

# What still needs filing

A read-only report. Do not file anything from this skill — if the user wants to
act on it, hand off to `/sonifide:register`.

## Steps

1. Read `~/.sonifide/agent.json` for `api_url` and `token`. Missing → tell them to
   run `/sonifide:connect` and stop.

2. `GET {api_url}/api/local-agent/tracks` with `Authorization: Bearer <token>`.

3. Group by release and report. For each track, show which of the five registries
   (`bmi`/`ascap`, `mlc`, `soundexchange`, `eco`) are `todo`, `submitted`, or
   `confirmed`, with the reference number where there is one.

4. For anything with unfiled registries, pull
   `GET {api_url}/api/local-agent/tracks/{id}/packet` and read its `readiness`.
   Report the blockers verbatim — each carries a `where` naming the exact
   Sonifide screen that fixes it. Do not paraphrase the fix; the wording is the
   same one the user's dashboard shows them.

   If a workspace has many tracks, pull packets only for the ones with unfiled
   registries, and say how many you skipped rather than implying full coverage.

   Blockers you can spot from the track list alone, without a packet:
   - **no ISRC** — blocks SoundExchange and cripples MLC matching
   - **no ISWC** — normal before the PRO filing; only a problem after
   - nothing filed at all on a released track

Lead with the summary line — "9 tracks, 4 fully filed, 5 with nothing at the MLC" —
then the detail. A wall of rows with no verdict is not a report.

5. End with the single highest-value next action and the exact command for it,
   e.g. `/sonifide:register 142`.

## Tone

Be concrete about money where it is true: an unregistered work at the MLC is
mechanical royalties accruing unmatched. Do not inflate it — no dollar estimates,
because you do not know their stream counts.

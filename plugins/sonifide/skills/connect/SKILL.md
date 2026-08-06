---
name: connect
description: Pair this computer with a Sonifide account using a one-time pairing code, so the other Sonifide skills can read filing packets and record registrations. Use when the user runs /sonifide:connect, pastes a Sonifide pairing code, or when another Sonifide skill reports that this machine is not paired.
---

# Pair this machine with Sonifide

Your job is to exchange a one-time pairing code for an agent token and store it on
this machine. Nothing else.

## What you need from the user

A pairing code that looks like `K7QP-3XRD`. They get it at
**sonifide.app → Registration → File with Claude → Generate pairing code**.

It is single-use and expires 15 minutes after it is generated.

If `$ARGUMENTS` contains something code-shaped, use it. Otherwise ask for it —
do not go looking for it anywhere on disk.

## Steps

**1. Check whether this machine is already paired.**

Read `~/.sonifide/agent.json` (on Windows, `%USERPROFILE%\.sonifide\agent.json`).
If it exists and has a `token`, say so and ask whether they want to re-pair before
overwriting it. Re-pairing is harmless — the old token keeps working until they
revoke it in Sonifide.

**2. Redeem the code.**

```bash
curl -sS -X POST "$SONIFIDE_API_URL/api/local-agent/pair" \
  -H "Content-Type: application/json" -H "Accept: application/json" \
  -d '{"code":"THE-CODE","label":"a short name for this computer"}'
```

Use `https://api.sonifide.app` unless the `SONIFIDE_API_URL` environment variable
is set (it is set only for Sonifide's own staging).

On Windows without Git Bash, use `curl.exe` from PowerShell with the same arguments.

A 422 means the code is wrong, already used, or expired. Say which of those it
probably is and tell them to generate a fresh one — do not retry the same code.

**3. Store the token.**

Create `~/.sonifide/agent.json` with permissions `600` (on macOS/Linux:
`chmod 600`). Contents:

```json
{
  "api_url": "https://api.sonifide.app",
  "token": "<the token from the response>",
  "expires_at": "<expires_at from the response>",
  "workspace": "<workspace.name from the response>"
}
```

**4. Confirm.**

Verify by calling `GET /api/local-agent/me` with
`Authorization: Bearer <token>`, then tell the user which Sonifide workspace this
machine is now paired to and when the pairing expires.

If `workspace.studio` is `false`, tell them agent filing needs a Studio plan and
point them at https://sonifide.app/upgrades. The pairing itself is still fine —
they just cannot pull filing packets yet.

Then tell them what to do next: click **File with Claude** next to any track in
Sonifide, or run `/sonifide:register` here.

## Rules

- **Never** print the token back to the user or paste it into a chat message.
  Confirm that it was stored; do not show it.
- **Never** commit `~/.sonifide/agent.json` to any repository, and never copy it
  into a project directory.
- This token reads that workspace's own catalog and writes registration outcomes.
  It cannot change billing, splits, passwords, or reach another account. If the
  user is worried about it, tell them they can revoke it any time from
  **Registration → File with Claude → Connected machines**.

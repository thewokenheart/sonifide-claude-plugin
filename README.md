# Sonifide for Claude Code

File your music registrations — your PRO, the MLC, SoundExchange, and the U.S.
Copyright Office — from your own machine, using your own registry logins, with the
metadata pulled straight from your [Sonifide](https://sonifide.app) account.

**Sonifide never receives an MLC, SoundExchange, PRO, or Copyright Office
credential.** Claude Code runs on your computer and works inside the browser
session you are already signed into. You stay the legal filer of record.

## Install

You need a paid Claude plan (Pro or Max), Chrome or Edge, and a Sonifide Studio
plan.

**1. Install Claude Code and sign in once**

```bash
# macOS / Linux
curl -fsSL https://claude.ai/install.sh | bash

# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

Then run `claude`, sign in, and send any prompt. That first real session is what
registers the `claude-cli://` link handler that the Sonifide button uses — it does
not happen on install alone.

**2. Add this plugin**

Inside a running `claude` session:

```
/plugin marketplace add https://github.com/thewokenheart/sonifide-claude-plugin.git
/plugin install sonifide@sonifide-tools
```

**3. Pair your machine**

In Sonifide, go to **Registration → File with Claude** and generate a pairing
code. Then, in Claude Code:

```
/sonifide:connect
```

Paste the code. It works once and expires in 15 minutes.

## Use

| Command | What it does |
| --- | --- |
| `/sonifide:connect` | Pair (or re-pair) this machine with your Sonifide account |
| `/sonifide:register` | File a track's registrations, one registry at a time |
| `/sonifide:check` | Follow up on filings awaiting a work number or ISWC, and write them back |
| `/sonifide:status` | Report what across your catalog is still unfiled |

Or click **File with Claude** next to any track in Sonifide — that opens a terminal
with the prompt already typed. You press Enter.

For the browser work, start with `claude --chrome` (or run `/chrome` once and
choose "Enabled by default").

## What the agent will and will not do

- It **stops before every submit** and shows you each field first.
- It **never enters payment details.** eCO's filing fee is typed by you.
- It **never handles a login or CAPTCHA** — it hands the browser back.
- It **never invents a value.** A missing IPI or ISRC becomes a question, not a guess.
- It **records honestly.** A filing it isn't sure about is recorded as `submitted`,
  never `confirmed`.

## What the pairing token can reach

Read your own workspace's filing data, and write a registration status plus a
confirmation number. That is the entire API surface. It cannot change splits,
billing, passwords, or any other account. Revoke it any time from
**Registration → File with Claude → Connected machines**.

The token lives at `~/.sonifide/agent.json` on your machine and nowhere else.
Never commit that file.

## The ISWC does not hold anything up

Your PRO assigns an ISWC one to three weeks after you register a work. It is an
output of that filing, not an input to the next one — the MLC treats it as
optional and backfills it, SoundExchange never asks for it, and the Copyright
Office doesn't use it. So the agent files everything in one session and Sonifide
reminds you to come back for the ISWC later. Run `/sonifide:check` when it does.

The only real prerequisite in the chain is the **ISRC**: SoundExchange matches
plays by it and nothing else, and MLC registrations without one collect nothing
even though they look correctly filed.

## Portals drift

Registry sites change their layouts. The reference files in
`plugins/sonifide/skills/register/references/` carry the screen-by-screen flows;
Claude reads the page it's actually looking at and stops rather than guessing when
something doesn't match. If a portal has changed enough to break the flow, open an
issue and we'll update the reference.

## Development

```bash
git clone https://github.com/thewokenheart/sonifide-claude-plugin.git
claude --plugin-dir ./sonifide-claude-plugin/plugins/sonifide
claude plugin validate ./sonifide-claude-plugin
```

Point at a non-production API with `SONIFIDE_API_URL`.

---

Sonifide is not affiliated with Anthropic, the MLC, SoundExchange, BMI, ASCAP,
SESAC, or the U.S. Copyright Office. Nothing here is legal advice.

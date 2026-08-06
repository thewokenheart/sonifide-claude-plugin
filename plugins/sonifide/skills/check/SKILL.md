---
name: check
description: Follow up on registrations that were submitted but never confirmed — look up the work in the PRO's public repertory or eCO, capture the assigned work number and ISWC, and write them back to Sonifide. Use when the user runs /sonifide:check, asks whether a filing came back, asks about an ISWC or a work number, or acts on a "time to check" reminder from Sonifide.
---

# Check on pending filings

A PRO registration doesn't come back the same day. The society reviews it and
assigns a **work number** and an **ISWC** days-to-weeks later; the Copyright
Office takes months. This skill goes and looks.

## What nothing is waiting for

Say this plainly if the user seems to be holding other filings back, because the
belief is common and it costs real money:

> **Nothing downstream needs the ISWC.** The MLC lists it as optional and
> backfills it later. SoundExchange is the master side and matches on ISRC only.
> eCO never asks for it. Waiting on an ISWC before registering at the MLC just
> means weeks of mechanical royalties accruing unmatched.

If they have unfiled registries, point them at `/sonifide:register` and do not
let the ISWC be the reason they wait.

## Step 1 — What is outstanding

Read `~/.sonifide/agent.json` for `api_url` and `token`, then:

```
GET {api_url}/api/local-agent/pending-checks
```

Each row carries the track, the registry, when it was filed, `days_waiting`,
whether it is `due`, how many `checks_done`, and what you are `expecting`.

If nothing comes back, say so and stop. Don't go looking anyway.

If `exhausted` is true on a row, Sonifide has stopped prompting for it. Tell the
user to contact the registry directly with their submission reference — several
weeks past assignment usually means the filing never landed, not that it is slow.

Filing less than about a week ago is normally too early. Say so rather than
opening a portal to find nothing.

## Step 2 — Look it up

Use the Chrome integration. The search happens in the user's own browser — most
of these searches are public, but a logged-in member portal shows more.

| Registry | Where to look | What you are after |
| --- | --- | --- |
| BMI | repertoire.bmi.com — public Songview search | BMI Work # and ISWC |
| ASCAP | ascap.com/repertory (ACE) | ASCAP Work ID and ISWC |
| SESAC | sesac.com repertory | Work ID |
| eCO | eco.copyright.gov/eService_enu/ → your case list | Registration number, or current status |

Search by **title plus writer name** — title alone returns other people's songs
with the same name. Confirm the writers on the result match the packet before
you take any number off it. If several results look plausible, show the user and
let them pick; a wrong work number is worse than none.

CISAC's ISWCNet (iswcnet.cisac.org) is the canonical ISWC lookup and is worth
trying when the PRO's own search is unhelpful.

Stop and hand the browser over at any login screen or CAPTCHA.

## Step 3 — Write it back

**The ISWC and the work number go to different endpoints.** The ISWC belongs to
the track; the work number belongs to the filing.

The ISWC, if you found one:

```bash
curl -sS -X POST "$API/api/local-agent/tracks/$TRACK/identifiers" \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"iswc":"T-034.524.680-1"}'
```

A `409` means an ISWC is already on file and differs from what you found. Do not
force it — show the user both and let them decide. A wrong identifier quietly
mismatches royalties for as long as it exists.

The filing status, with `is_check` set so the follow-up schedule advances
instead of restarting:

```bash
curl -sS -X POST "$API/api/local-agent/tracks/$TRACK/registrations" \
  -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"type":"bmi","status":"confirmed","work_number":"12345678","is_check":true}'
```

- Found the work, live in the repertory → `confirmed`, with `work_number`.
- Not there yet → post `status: "submitted"` with `is_check: true` anyway. That
  records the look and pushes the next reminder out, so the user is not nudged
  again tomorrow about something you just checked.

## Step 4 — Say what it changes

Report per filing:

- confirmed, with the work number and ISWC
- still pending, with how long it has been and when to look again
- exhausted — time to contact the registry

Then the one thing that actually follows from an ISWC arriving:

- **MLC already filed** → adding the ISWC to that registration improves how
  streams match to the work. Offer to do it now; it is a small edit in the
  portal, not a refiling.
- **MLC not filed** → file it now, with the ISWC. Offer `/sonifide:register`.
- **Nothing else outstanding** → say so and stop.

Never imply the ISWC unblocked something. It didn't.

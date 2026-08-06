# ISWC — and why nothing waits for it

The ISWC identifies the **composition**. It is assigned by the songwriter's
society (through CISAC) after the work is registered — it is an **output** of the
PRO filing, never an input to it.

## The expensive misconception

It is natural to assume the chain is:

> ISRC → PRO → *wait for the ISWC* → MLC → SoundExchange → eCO

It isn't. Only one of those arrows is real.

| Registry | Needs the ISWC? |
| --- | --- |
| The MLC | **No.** Optional field. Improves matching; the MLC backfills it later. |
| SoundExchange | **No.** Master side — matches on ISRC and never asks. |
| eCO | **No.** Doesn't use ISWC at all. |
| Songtrust | **No.** Optional. |

A society takes one to three weeks to assign one. Holding the MLC registration
for it means one to three weeks of mechanical royalties accruing with no
registered owner attached — which is the exact loss this product exists to
prevent.

**So: submit the PRO filing, do not wait, file everything else, backfill later.**

The packet's `plan` object states this in machine-readable form:
`plan.sequence[].blocked_by` lists only genuine hard dependencies, and the only
one in the whole graph is the ISRC. `plan.iswc_note` carries the standing
correction. Trust that over any remembered ordering.

## What the ISRC actually blocks

This one is real, and it is worth being strict about:

- **SoundExchange** matches plays by ISRC and nothing else. No ISRC, no payment,
  ever.
- **The MLC** accepts a work without an ISRC, but DSP usage reports arrive keyed
  by ISRC — so the registration looks fine and collects nothing.

Generate the ISRC first. See `isrc.md`.

## Getting the ISWC afterwards

Sonifide books a follow-up when a PRO filing is recorded as `submitted`, and
reminds the artist on a backing-off curve (roughly 10, 14, 21, 30 days). The
lookup itself is `/sonifide:check` — it searches the society's public repertory
in the artist's own browser and writes the ISWC and work number back.

There is no server-side polling, and that is deliberate: no licensed public API
exists for ISWCNet, BMI's or ASCAP's repertory, and scraping registry sites from
a datacenter is the fragile, terms-hostile automation this whole design avoids.
The lookup runs where the filing ran — on the artist's machine, under their own
session.

## Format

`T-034.524.680-1` — a `T`, nine digits, and a check digit. Registries vary on
whether they want the punctuation; enter it as the portal displays it.

Once you have it, put it on the track in Sonifide (`POST
/api/local-agent/tracks/{id}/identifiers`), and add it to the MLC registration
if that filing already went in. Both are small edits, not refilings.

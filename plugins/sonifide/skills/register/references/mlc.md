# The MLC — Notification of Works

**Portal:** https://portal.themlc.com

Collects US **mechanical** royalties from streaming and downloads. If a work is
not registered here, the money accrues unmatched and eventually gets distributed
by market share to everyone else. This is the single highest-value filing for most
independent writers.

## Prerequisite

The user needs an MLC account as a **self-administered writer** or through a
publisher/administrator. Every writer on the work must be represented by someone
on the account. If they have never signed up, that is a separate onboarding — stop
and tell them, and point them at the "Members" signup on the portal.

If `artist.mlc_id` is present in the packet, they already have one.

## Shares are 100% here

Unlike the PRO screens, MLC writer shares total **100%** — the raw split
percentages straight from `splits`. The `guides` MLC entry already reflects this.
Do not carry the doubled PRO numbers over.

If the shares in the packet do not total 100%, stop. Do not round them to fit.

## Screens

**1. Work details** — Title, alternate titles, duration, ISWC. Leave ISWC blank if
Sonifide doesn't have one; the MLC will match it later off the PRO registration,
which is why the PRO gets filed first.

**2. Writers** — One row per writer: legal name, IPI, role, share, and their
publisher/administrator. A writer with no publisher on the account will block
submission — the MLC needs to know who to pay.

**3. Recordings** — Add the **ISRC**. This is what links streams to the work. A
work registered with no ISRC still collects nothing, because the DSP reports
arrive keyed by ISRC. If the ISRC is missing, generate it first (see `isrc.md`)
and come back.

Recording title and release date come from the packet's `track` and `release`.

## Bulk path

If the release has many tracks, the portal's **Bulk Upload** accepts a
spreadsheet. Fetch it rather than typing each work:

```
GET {api_url}/api/local-agent/releases/{release_id}/export/mlc-xlsx
```

There is also a CWR v2.2 file (`mlc-cwr`), but the MLC only accepts CWR from
registered senders — do not offer it unless the user says they are one.

Even on the bulk path, show the user what is in the file before uploading it.

## Recording the result

`type: "mlc"`. Capture the MLC work number. Registration is usually accepted
immediately but reviewed — use `submitted` unless the portal shows the work as
registered, then `confirmed`.

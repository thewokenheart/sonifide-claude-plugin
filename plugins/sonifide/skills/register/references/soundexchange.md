# SoundExchange — recording repertoire

**Portal:** https://www.soundexchange.com (member login → SXDirect)

Pays the **featured performer** and the **master owner** for non-interactive
digital plays — internet radio, satellite radio, cable music channels. This is the
recording side, not the song. It is the money most independent artists never
collect, because nobody told them it exists separately from their PRO.

## Prerequisites

Two separate registrations, and the user may have one without the other:

- **Artist (performer) account** — collects the performer's 45%.
- **Rights owner (label) account** — collects the master owner's 50%. If they are
  self-released, that is also them.

If `artist.soundexchange_id` is in the packet they have an account. If not, sign-up
is a prerequisite and takes a few days for verification — say so and don't try to
rush past it.

## The ISRC is mandatory

SoundExchange matches plays by **ISRC**, full stop. If the packet's
`track.isrc` is empty, stop and generate one first (`isrc.md`). Submitting a
recording without an ISRC produces a row that will never match a play.

## Fields

| Field | Source |
| --- | --- |
| Recording title | `track.title` |
| Featured artist | `artist.stage_name` — the name as credited on the release |
| ISRC | `track.isrc` |
| Duration | `track.duration` |
| Album / release title | `release.title` |
| Release date | `release.release_date` |
| UPC | `release.upc` (optional but helps matching) |
| Rights owner | `artist.publisher_name` or the legal name — whoever owns the master |
| Rights owner share | 100% if self-released |

If the master is co-owned (a label deal, a producer with points on the master),
the shares must reflect that. Ask; do not assume 100%.

## Bulk path

For a full release, SXDirect accepts a repertoire file. Fetch the DDEX ERN:

```
GET {api_url}/api/local-agent/releases/{release_id}/export/soundexchange
```

Caveat worth telling the user: this file is generated to the DDEX ERN shape but
has not been validated against SoundExchange's live intake. If the portal rejects
it, fall back to entering the recordings by hand — that always works.

## Recording the result

`type: "soundexchange"`. Capture whatever reference the portal returns. Repertoire
submissions are queued for matching rather than instantly confirmed, so `submitted`
is usually the honest status.

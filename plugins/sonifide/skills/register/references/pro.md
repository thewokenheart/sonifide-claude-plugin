# PRO — BMI / ASCAP / SESAC (the composition)

Registers the **song**, so performance royalties (radio, TV, venues, streaming
performance) flow to the writers and publishers. This is not the recording — that
is SoundExchange. File the PRO **first**: it assigns the work code that the MLC
and everyone downstream reference.

Which one depends on `artist.pro` in the packet. A writer belongs to exactly one
PRO. If `artist.pro` is empty, stop and ask — do not default to BMI just because
it is common.

| PRO | Portal | Notes |
| --- | --- | --- |
| BMI | https://repertoire.bmi.com | "Register a Work" |
| ASCAP | https://www.ascap.com/ace → Member Access | "Register a Title" |
| SESAC | https://www.sesac.com | Invite-only; same shape |

## The trap that gets filings wrong: 200% vs 100%

PROs split the song into two pools — a **writer's share** and a **publisher's
share** — and each pool totals 100%. Depending on the screen, a PRO may ask for
writer shares that total **200%**.

The packet has already done this arithmetic for you. In each `guides` entry for
the PRO, the writer rows carry the **PRO-facing** share, not the raw split
percentage. Use the number in the guide, not the number in `splits`.

- **No publisher on the work** — the writers self-administer, so their writer
  shares are doubled to total 200%.
- **A publisher exists** — writer shares total 100% and the publisher rows carry
  the matching 100% publisher share.

If what the form is showing doesn't add up to what the guide says, stop and ask
the user rather than adjusting a number yourself.

## Screens

**1. Work info** — Title (exactly as in the packet, no featured-artist suffix),
alternate titles, duration. Leave **ISWC blank**: the PRO assigns it. Note it down
afterwards and tell the user to save it in Sonifide.

**2. Other works / derivation** — "Is this an arrangement of a public-domain work
or a foreign lyric?" For an original song this is No. The packet's
`public_domain` flag tells you which. Samples: if the work contains a cleared
sample, the underlying writers must be added as writers with their shares —
ask the user rather than assuming.

**3. Writers and publishers** — One row per writer: name (**legal** name as
registered with the PRO, not a stage name), role (Composer/Author, or CA), the
IPI, the affiliation, and the share. Then publisher rows if any.

A writer with no IPI cannot be added. If an IPI is `missing`, stop: they have to
be a registered member of a PRO first. That is a real prerequisite, not a
formality.

**4. Artists and recordings** — Performing artist, and the ISRC once it exists.
Optional, but linking it here helps matching later.

## Recording the result

Capture the **work number / work ID** the PRO returns. Post it back as
`type: "bmi" | "ascap"` (there is no `sesac` type — if the user is a SESAC writer,
record the outcome in the note field of whichever registry you file next, and tell
them, rather than filing it under the wrong PRO).

Status is `submitted` on acceptance; PROs review before the work is live, so only
use `confirmed` once the work actually appears in the public repertory search.

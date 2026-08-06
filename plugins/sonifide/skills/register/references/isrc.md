# ISRC — the recording code

**Not a filing.** Nobody registers an ISRC; the owner of a registrant prefix
assigns them. But it is a prerequisite for SoundExchange and the MLC, so it comes
first.

## Structure

`US-XXX-YY-NNNNN`

| Part | Meaning |
| --- | --- |
| `US` | Country of the registrant |
| `XXX` | The registrant's 3-character prefix — **assigned once, for life** |
| `YY` | Two-digit year the code is assigned (not the release year) |
| `NNNNN` | Sequential designation, 00001 upward, restarting each year |

## Where the prefix comes from

https://www.usisrc.org — a one-time fee (about $95 at time of writing; check the
site, don't quote a stale number). Approval is not instant.

If the user has no prefix, this is a real blocker, not something to work around.
Two honest options to offer:

1. Get a prefix from usisrc.org and come back.
2. Use the ISRCs their distributor assigns. DistroKid, TuneCore and CD Baby all
   issue codes automatically. Those codes are perfectly valid — the only downside
   is the prefix belongs to the distributor, not the artist.

Never invent a prefix, and never reuse an existing code for a different recording.

## Assigning

A code identifies **one specific recording**. A remix, a radio edit, a live
version, and an instrumental each need their own. A remaster of the same master
does not.

Once you have the code:

1. Tell the user the exact string.
2. Tell them to save it on the track in Sonifide (Metadata → the track's ISRC
   field). The agent token cannot write metadata — only registration outcomes —
   so this step is theirs.
3. Only then move on to SoundExchange and the MLC.

There is no `isrc` registration type to post back. Do not fabricate one.

# U.S. Copyright Office — eCO

**Portal:** https://eco.copyright.gov/eService_enu/
(The trailing path matters — the bare domain redirects to eservice.eco.loc.gov and
answers 403 in a real browser, so it is a dead end, not a login page.)

Federal copyright registration. This is the strongest ownership evidence in a
dispute, and it is a prerequisite for suing over infringement in the US. It also
costs money and involves a **sworn declaration** — which is why the rules below
are stricter than for any other registry here.

## Non-negotiable

- **The user signs it, not you.** The certification screen is a legal statement
  made under penalty of perjury. Read it to them, then hand the browser over.
  Never tick that box yourself.
- **The user pays it.** Navigate to the payment step; stop there. Do not touch
  card fields, do not fill Pay.gov.
- **Never guess the author, the claimant, or the year.** These determine who owns
  the registration.

## PA or SR?

Two different works live in one recording:

- **PA** (Performing Arts) — the **song**: melody and lyrics.
- **SR** (Sound Recording) — the **recording**: the specific master.

If the user wrote the song *and* owns the master, a single SR application can
cover both, but only when the claimant is identical for each. Ask which they want
rather than deciding. If unsure, filing both separately is the conservative answer
and costs two fees — say that plainly so it's their choice.

## Standard Application flow

1. **Register a Work → Standard Application.**
2. **Type of work** — Work of the Performing Arts, or Sound Recording.
3. **Titles** — `track.title`. eCO wants Roman letters and Arabic numerals; if the
   title has other characters, transliterate and note the original in a
   "Note to Copyright Office".
4. **Publication** — Published or unpublished, and the date of first publication.
   Use `release.release_date` if the release is out. If it isn't out yet, it is
   unpublished. Getting this wrong is a common defect.
5. **Authors** — legal names from `artist.legal_name` and the writers in `splits`.
   Set what each author contributed (music, lyrics, sound recording, performance).
   Work-for-hire only if that is genuinely the arrangement — ask.
6. **Claimants** — who owns the copyright now. Usually the same as the authors.
   If ownership was transferred, eCO asks how; that is a question for the user.
7. **Limitation of claim** — samples, interpolations, or previously registered
   material get excluded here. Also where **AI-generated material** must be
   disclaimed. If any part of the work was AI-generated, it must be disclosed and
   excluded from the claim. Ask directly; do not infer from the packet.
8. **Certification** — hand over to the user.
9. **Payment** — hand over to the user.
10. **Upload the deposit copy.**

## Deposit copy

Fetch the pre-built bundle — the audio inside is already renamed to the Office's
`NN_Title.ext` convention, which is the fiddly part:

```
GET {api_url}/api/local-agent/releases/{release_id}/export/eco
```

Save it to a temp path and attach it. Open the checklist inside it if you need the
per-track file list.

## Group registration

Up to 10 unpublished works by the same author(s) can go in one Group
Unpublished Works application — one fee instead of ten. If the release is
unpublished and the authorship is identical across tracks, tell the user this
before filing them one at a time. It is a large, real saving.

## Recording the result

`type: "eco"`. Capture the **Service Request number**. Status is `submitted` —
the Copyright Office takes months to issue a certificate, so it is never
`confirmed` on the day you file. Record the fee actually paid.

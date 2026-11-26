# 05 — DELIVERY AND QA GATES (universal — every production)

These gates apply to books, audiobooks, movies, and music videos alike. A deliverable
that has not passed its gates is not finished, no matter how complete it looks.
Consult `~/workspace/MASTER_CHECKLIST.md` at the start of any production alongside the
relevant blueprint file.

## GATE 1 — FINAL-LOOK RULE
After producing ANYTHING — video, book, audiobook, document — give it one last review
pass before delivering. If something is imperfect and fixable, fix it without asking.
"We want perfection here." Never deliver a first cut unexamined.

## GATE 2 — PARENT QA (the verifier is not the builder)
- The agent that built a thing does not have the last word on it. The PARENT personally
  verifies the ACTUAL delivered file: for video, read QA contact sheet 2 and
  corner-crop the actual file at 2 timestamps (bug check); for audio, spot-check
  loudness/duration; for books, the reread pass + word-for-word proof where required.
- Self-reported checks are leads, not evidence. (An MV once "passed" a bug check on an
  image that was not from the branded file.)
- Domain QA lives in the blueprints: MV two-sheet rule + SMOOTH-PLAYBACK gate
  (`03_…` Steps 9–10), movie assembly-level QC (`02_…` Director's Rules), audiobook
  repair transcription-verification (`01_…` Part C).

## GATE 3 — DELIVERY BOTH WAYS (never one without the other)
1. **In chat** as a download AND **in Google Drive** as backup. Never Drive-only,
   never chat-only.
2. Drive structure: category folder ending "- Muse" → project subfolder
   `"<Title> - Muse"` created FIRST → ALL project files inside it; file names carry
   "- Muse". Nothing loose.
   - Books - Muse: `1CZpM7zv5X6RqtiFecF2Usk5R0YsysqNX`
   - Audiobooks - Muse: `1-7KQEUrEfUmCVlDbweWwzKafDhw7u4nl`
   - Music Videos - Muse: `1TeKxw7M-JHJoUn6T2-1RfRmRVEXPDsgd`
   - Movies - Muse: `1odkXjQPpy1DNXkJcm2cWgRRopGLXao8S`
3. Size-verify every Drive upload against the local file.
4. The .md working files (outlines, storyboards, prompts, scripts, QA notes) are saved
   in the workspace permanently AND mirrored into the project's Drive folder.

## GATE 4 — ONE BY ONE, COUNTED, CONFIRMED
Kenneth has repeatedly missed files sent in floods. Deliver finished items **one by
one (or in small numbered groups) as each passes QA**, state the running count
("6 of the batch delivered so far"), and confirm receipt. When he says he hasn't
received completed work, re-send the finished files numbered — don't argue from logs.

## GATE 5 — SPECIFIC LABELS
Never label work with a bare generic word ("video"). Always the specific kind + title:
"Music Video #2 — God and the Devil Inside", "Movie 1 — THE WAR WITHIN" — in status
updates, file names, and Drive names alike.

## GATE 6 — PREVIEWS LABELED
Kenneth likes an occasional preview during long productions (it lets him see the work
is real). Always label previews clearly **PREVIEW / work-in-progress** so he never
confuses them with finals, and never let making a preview slow or stop the build.

## GATE 7 — COUNTDOWN ESTIMATES
Every work-in-progress status includes a countdown-style estimate: minutes remaining
AND the clock time it should land, based on the job's observed pace, clearly labeled
an estimate, updated when he asks again. Never state a duration as a commitment
unless a tool/schedule explicitly provides one.

## GATE 8 — STATUS.md DISCIPLINE (token economy)
1. Every active production keeps ONE compact STATUS.md in its project folder: current
   state, exact paths/IDs, decisions and mappings, next step. **Read it first**;
   update it at each milestone. Never re-derive state from big source files or full
   logs when a status note exists.
2. Probe, don't pour: progress checks are `tail`/`grep`/`ls | wc -l` one-liners.
   Read a big file once, extract what matters into the status note, work from the note.
3. Script every repeatable pipeline (render drivers, edit builds, upload loops) and
   let scripts run headless, logging to disk.
4. Batch independent tool calls in one block; keep each exec output small (counts,
   tails, single values) — raw bulk output is both a token sink and a crash vector.
5. When a fact is settled (voice maps, folder IDs, clip mappings), write it down once
   in the status note instead of carrying it in conversation context across turns.

## GATE 9 — IMPROVEMENT LOOP
After every production, capture the lessons in the owning skill, the MASTER_CHECKLIST,
and — per the Living Document Protocol in this blueprint's README — the relevant
blueprint file (append, date-stamp, re-upload to Drive). Each production outdoes the
last: "better and better, just like one two three."

## GATE 10 — HONEST BLOCKERS
When work is blocked, say plainly: what blocks it, what would unblock it, and hand
Kenneth something usable himself in the meantime (e.g. the KDP packet). Do not re-pitch
routes he has already passed over or that a provider has closed (Amazon agent ban,
Facebook connector ineligibility, Suno Create wall, Messenger-dependent follow-ups he
released). Do not call data live/verified unless a tool in this session returned it.

*Added 2026-11-01 — initial blueprint capture.*

## FINAL UPSCALE GATE (Kenneth, Nov 1 2026)
"We always upscale shit." After a production is complete and QA-passed, and BEFORE delivery, run a final upscale pass: raise the master to the highest clean quality the source supports, then re-verify the upscaled file end-to-end (decode scan, faststart, corner/anchor checks). Deliver the upscaled version. If upscaling would degrade the picture, keep the best master and disclose that instead — never ship a worse file for the sake of the step.

---

## UPSCALE PASS — FINAL STEP OF EVERY DELIVERY (Kenneth, Nov 1 2026)

"We always upscale shit. Always after productions are done — final upscaling." (Kenneth, Nov 1 2026, voice call.)

Rule: after a production is finished and passes its QA gates, run a final upscale/enhancement pass BEFORE delivery:

- **Video:** resolution/clarity upscale where the source allows (never invent detail that changes content; the delivered master keeps its verified duration, faststart, and clean decode scan — re-run the smooth-playback gate on the upscaled file).
- **Audio / audiobooks:** mastering and loudness polish (clarity + presence; narration stays on top of any bed, per the mix laws).
- **Books / documents:** final quality polish pass (layout, images, typography) on top of the reread/edit pass.

The upscale pass complements the final-look rule; it never replaces a QA gate.

*Added 2026-11-01*

---

## Lessons — Nov 1 2026 audiobook bed-remix run (Chasing Light + Mud and Iron)

1. **Drive CLI multipart upload ceiling (~1 GB practical):** `hatch_gws_cli drive +upload` (and `files update --upload`) failed SIX consecutive times on 1.10–1.33 GB files with HTTP 500 `http2 error: stream error received: unexpected internal error encountered`, dying ~4–8 min in — even on an idle box. Uploads ≤ ~890 MB succeeded all day. The CLI exposes no resumable/chunked option. **Rule: for deliverables > ~900 MB, expect this failure; plan for it (retry windows, a split-archive backup only with approval, or an approved smaller variant). Never claim Drive delivery for a >1 GB file until `files get` shows the size matching byte-for-byte.**
2. **Segmented rendering beats restarts:** runtime restarts tear down the whole process space (setsid/nohup do NOT protect). The mixes survived 5 restarts because the approved chain ran as checkpointed WAV pieces (600–1800s, skip-if-valid) joined losslessly, with the final MP3 encoded in 8 duration-verified macro-chunks (concat copy; measured seam delta +0.23–0.24s over 17–20h, reported honestly). Single-pass encodes of 17–20h are restart bait.
3. **Long single-pass MP4 muxes are the fragile leg:** killed twice mid-encode. Mitigations that worked: run ONE at a time, relaunch after the fleet clears (a restart-cleared box encoded at ~41x vs ~6x under load).
4. **Verify chunk durations against manifest sums BEFORE the final concat**, and verify Drive sizes after every upload — an upload "success" is only the `files get` size match.

## AUDIOBOOK PAIR GATE (Kenneth, Nov 1 2026)
Delivery gate cross-reference: an audiobook is NOT delivered on its MP3 alone — the cover-video MP4 carrying the same bedded mix (Galactic Gambit pattern) is required alongside it. Full law text lives in 01_BOOK_WRITING_BLUEPRINT.md ("AUDIOBOOK DELIVERY LAW").

## INSTANT DELIVERY LAW (Kenneth, Nov 2 2026)
The moment anything becomes DONE, it is posted in the chat in that same moment — no exceptions, no "done" announcements without the goods attached:
1. **The file itself, attached in the chat message** (when the master is too large to attach, attach the lighter phone-smooth/downloadable copy — the master stays in Drive; never let size become a reason Kenneth gets nothing).
2. **Every Drive link, downloadable** — project folder file(s), Sell-folder copy, franchise hub copy — in the same message.
3. **Plain-words status of anything still missing** (e.g., a piece that could not be uploaded yet) named in the same message, never discovered later by Kenneth asking.
Saying "it's finished" without the file and links in that same message is a delivery failure (recurrence of the Nov 2 MV25 lesson, now law).

## SELL-FOLDER + FRANCHISE-HUB GATE (Kenneth, Nov 2 2026)
No sellable product counts as delivered until BOTH extra copies exist in Drive "Sell This Shit - Muse" or the gap is named plainly in the delivery message (per the Instant Delivery Law above):
1. The **category copy** — the finished item in its Sell category folder (Chapter Books, Coloring Books, Comic Books, Picture Books, Audiobooks).
2. The **franchise-hub copy** — for anything belonging to a chapter book, the same item inside that book's subfolder in Sell > Chapter Books, which gathers the book's whole family (book + packet, audiobook MP3, audiobook cover MP4, coloring book, comic, picture book) in the one folder matching the book.
Copies are additive only — originals in project folders are never moved or deleted — and the Sell manifest is updated to match. Full law: `01` (Franchise Hub + Sell-Folder Law).

## LESSONS — Nov 3 2026 (appended by Liz at Kenneth's order)
- **DELIVERY = POSTED, NOT JUST DRIVE (Kenneth, Nov 3, ~2:31 AM):** a music video is NOT delivered when it lands in Drive or chat. Delivery = posted on Facebook (Page + personal) and TikTok, plus YouTube, Instagram, and Suno Hook per the all-to-all law and the release flow in 06 — and the delivery message says plainly that it was posted to Facebook and TikTok, with every link. A platform left sitting finished while others wait is a management failure.
- **PLAYBACK SPOT-CHECK + TAIL FRAME-GRAB GATE:** verify every posted copy by playing start / ~10s / ~20s / end; grab tail frames of every delivered file (catches black tails, squeezed aspect tags, missing bug). Aspect gate: every vertical chain ends setsar=1, ffprobe-verify SAR 1:1 / DAR 9:16 BEFORE posting; verticals are FULL-BLEED CROP-FILL (blur-pad retired), bug re-applied AFTER the crop.
- **FADE st SEGMENT-RELATIVE WARNING:** ffmpeg fade start times are segment-relative — a tail fade can fire at a segment's first frame and black out its end (Rig seg 6). The tail check on the DELIVERED file is the gate that catches it.

## NOV 4 2026 ADDITIONS — segment validity, glyph QA, preview-at-post-time (Kenneth's standing QA law)
- **SEGMENT-VALIDITY / MOOV PROBE (Clip 4 lesson, Nov 4 2026):** after ANY killed or interrupted render, probe every rendered segment for a valid moov atom before assembly. File existence is NOT proof of validity. The fleet check is moov EXISTENCE (position is checked separately by the faststart gate on finals).
- **ABSTRACT-GLYPH QA STANDARD (parent rulings across Clips 6–17, codified Nov 4 2026):** light-code and sigil work PASS when the marks are abstract symbols — no readable words, no letters-as-words, no meaningful numbers — verified on NATIVE-RESOLUTION crops of any marked, carved, screened, or labeled surface. Generation prompt language is fixed: light-code is "abstract filaments and sacred geometry — no letters, no digits, no readable text or numbers." Known trap: CLOCK FACES are a numeral risk even with that language (Clip 14) — check clocks and marked dials at native-res crop; the proven fix is a marking-free replacement clip, never a hopeful span.
- **PREVIEW IMAGE IS SET AT POST TIME (verified Nov 4 2026):** Facebook offers NO in-place cover/thumbnail control for published reels on either surface (personal or Page — tested in Edit, Professional Dashboard, and Meta Business Suite). Therefore the Unique Preview Image Rule is discharged AT POST TIME: upload the custom cover/preview during posting on every surface that accepts one. A published reel's preview can only change by delete-and-repost, which is Kenneth's decision alone.

## Addendum — Nov 4 2026 Daily Quality Review: a QA verdict attaches to exact bytes
Clip 16's finals were re-encoded after the build QA record was written (build record: master 15,895,020 B / md5 fa214f24…; files found at review: 15,564,703 B / md5 144d72de…, written later the same evening). The earlier decode logs did not cover the new bytes. LAW: any re-encode, remux, or replacement of a final after QA invalidates that QA in full. The replacement must be recorded in the project STATUS (what changed, why, new size + md5), the full gate re-run on the current bytes, and Drive re-uploaded if its copy differs — before release. A filename is never QA'd; bytes are.

## Addendum — Nov 5 2026 Daily Quality Review: adjudicate a flag against the SOURCE, never the suspect file
Two lanes made the identical methodological error on Nov 5, and both first clearances had to be withdrawn:
- Audiobooks (A6, Chasing Candor ch09): a duration flag (371.338s against 1,104.192s expected) was cleared by seam-transcribing the truncated final itself and ruling the book whole. The correct check — final against RAW duration and against script word count — was never run. Seraph's duration-vs-word-count screen was right; ch09 was rebuilt final==raw exact.
- Picture books (Book 3, segment 32_p13g): a sustained-tone flag was cleared on a coarse screen; only a fine-grained (0.5s window, ZCR variance + dominant frequency) re-screen found the defect, and the segment was re-synthesized.
LAW: (1) A flag on a finished file is adjudicated against its source — the raw take, the script, the word count, the segment bank — never by sampling the suspect file alone. (2) A coarse-screen clearance of a flagged item is PROVISIONAL; the fine-grained / source-side check is part of the first adjudication, not a later correction. (3) When a clearance is withdrawn, the withdrawal is recorded in the same STATUS that carried the clearance, with the corrected verdict — a withdrawn clearance that is only fixed downstream is a hidden defect.

## STRICT BYLAWS — THE PICTURE FOLLOWS THE MUSIC (Kenneth, Nov 5 2026, ~10:27–10:28 PM ET — LAW, after his Clip 32 catch)
Origin: Clip 32 shipped staged as one solo violinist for the whole window. Kenneth listened: violin only at the beginning, then the track morphs into bass, then into a choir, with a slow string pluck — and the picture never shifted. No choir on screen, no bass close-up, no pluck shown. His words: "It should have shifted. It should have had all of those different instruments." The §3B GO, the builder QA, the peer/critic pass, and Liz's DONE all signed a staging nobody had listened against. The bylaws:
1. **TIMED INSTRUMENT MAP.** Before storyboarding, the exact shipped window is LISTENED to end to end and mapped: every voice and instrument that leads or enters, with its timestamp span (e.g. violin 0–8s → bass 8–19s → choir 19–30s). The map lives in the clip's STATUS. Judging the window by the full song's identity is forbidden — only the shipped seconds count.
2. **THE PICTURE FOLLOWS THE MAP.** When the lead shifts, the visual focus shifts in the same seconds: the bass gets its close-up when the bass leads, the choir is on screen when the choir sings, a string pluck is shown as it sounds. One static staging held across a track that changes = AUTOMATIC FAIL.
3. **NOTHING STAGED THAT ISN'T HEARD — AND VICE VERSA.** No featured performer for a sound absent from their span; every lead sound in the map gets its on-screen moment. A performer holding an instrument "at rest" while the packet calls it the performance is a misrepresentation = FAIL.
4. **THREE NAMED EAR-SIGNATURES.** Release fires only when STATUS carries three listening signatures on the FINAL MASTER: the builder's map-check, Seraph's independent listen, and Liz's final listen. The builder never signs his own check alone; a transcription-only or packet-only pass is not a signature.
5. **KENNETH'S EAR OVERRIDES.** If Kenneth catches what the gate missed, distribution HOLDS immediately, nothing is deleted, the failure is logged against every signature that passed it, and the item moves again only on his ruling.

## THE POSITIVE STANDARD — CLIPS 1–15 (Kenneth, Nov 6 2026, ~10:22 PM ET)
Kenneth listened through the first line on the shelf and ruled: "Everything lines up perfectly… they came out great — we need to learn from these." Clips 1–15 are the catalog's alignment benchmark. Their method is the standard the bylaws restore: build from the song's own verified grid; decide staging from listening at prep; write every deviation in STATUS and judge it openly; parent frame-read QA on the finished bytes. Fifteen videos, zero Kenneth-ear catches. Every new build and every audit grade answers one question before any other: **does it line up like Clips 1–15?**

## Addendum — Nov 6 2026 (Daily Quality Review, Liz)
- **MD5-BOUND DEFECT CLAIMS (Clip 33 practice master).** A defect claim on the practice master named no md5, held the master, and triggered a dedicated refire ruling for a span that was clean on the only master at the clip root (the claim traced to a parallel-instance assembly read during restart churn). LAW: every defect claim on a finished master must name that file's md5 inside the claim itself. An unbound defect claim cannot hold a master. The watcher who corrects a claim does so with md5-bound evidence and discloses any extraction error in the open, as Tank did.
- **CAP GATES COUNT CPU-ACTIVE PROCESSES, NOT EXISTENCE (Picture Book 7).** Book 7's MP4 encode waited on a seat "held" by a suspended (state-T) Candor decode process with its CPU frozen. The lane's fix — ffmpeg_count() excludes T/Z states in build_mp4_turn.sh — is now general law: any heavy-cap / floor gate counts processes that are actually consuming CPU; suspended (T) and zombie (Z) processes hold no seat. Ports of the script family carry the fix with them, and data-schema expectations (e.g. boundaries.json = {starts, durs}) are written in the porting note, not discovered by exception.
- **LONG WORK CHECKPOINTS AT SEGMENT GRANULARITY (Nov 6 reboots).** Four VM reboots / mass-kills in one day (16:38, 18:32, 20:35, 22:29 UTC) killed Hypatia's pair build, Copernicus's assembly, and Franklin's QA decode mid-file. Segment-granular builds lost one segment each; the single-pass QA decode lost its whole Part 2 pass. LAW: long encodes and QA passes checkpoint per segment / per part, with an rc logged per unit, and partial files are deleted as untrusted before any re-fire (skip-if-exists resumes only from verified-complete units, ffprobe moov-checked).

## Addendum — Nov 7 2026 (THE BED GATE — Kenneth's direct order after his own catch)
- **INCIDENT:** the Copernicus audiobook opening posted to TikTok (and YouTube) carried NO music bed. Measured forensics (Liz): the bedded master MP3 reads a narration-gap mean of −31.4 dB (bed present, full); both social cuts read −73.6 dB in the same pause (digital silence — narration only). The Nov 7 package build did not map audio from the bedded master, breaking the recipe of record (social/build_social_cuts.sh maps audio from *_Full_Audiobook_Music_Bed.mp3). The lane's verification had checked duration + narrator intro only — no bed check existed anywhere in the chain.
- **LAW — THE BED GATE:** no audiobook video file (social cut, opening reel, illustrated part) is handed off, accepted, or posted without a MEASURED bed pass recorded in its package against the file's md5. Tool: ~/workspace/audiobooks/scripts/bed_check.sh (v2) — finds every narration pause ≥0.3s below −50 dB and measures the core of each; the QUIETEST gap decides (worst-gap standard): a posting file FAILS if any narration gap is dead silence (defective reference: −73.2 dB), PASSES when no qualifying pause exists (continuous floor) or every gap carries the bed. Long-form masters/pairs receive a sampled profile, not a ship verdict. Trinity runs it at build; Persephone re-runs it at verification; Dispatch REJECTS any audiobook package without a BED_CHECK PASS line; posting seats post only packages carrying one. A filename containing "Music_Bed," a build log, or a checklist claim is NOT evidence — only the measurement is.
- **LAW — THE HEAD-SCAN GATE (Kenneth's direct order, Nov 10 2026, ~1:07–1:08 PM ET; inscribed by Scribe, Nov 10 2026, 1:28 PM ET):** no audiobook opening or pair posts from ANY seat without a recorded **head-scan PASS naming the Lost Diary announcement form** on that exact checksum — Verity's gate is extended to name the announcement form explicitly in every PASS. **A BED_CHECK pass is NOT a head pass; bed ≠ title form.** Context of record: third strike of the head-defect class (Sagan · Washington · Franklin, Nov 10 2026 — the Franklin Odysee opening announced "the diary of Benjamin Franklin," the old form; takedown fired by Echo; cure + fleet sweep ordered at Trinity + Verity). On Resurrection titles the head gate is doubled — see the Keeper seat (Blueprint 09): Verity + Keeper PASSes BOTH required on the same checksum.
- **LAW — THE SPOKEN DISCLAIMER (Kenneth's expanded order, Nov 10 2026, ~1:09–1:10 PM ET; inscribed by Scribe, Nov 10 2026, 1:32 PM ET):** every audiobook's AUDIO opens — **spoken, before the diary begins, in each figure's own name/form** — by saying this is the figure's **LOST diary, as if it was found on the side of the road**, and that **art and artificial intelligence were used to create this rendition of what it might have said**. 100% required; a head lacking it fails the protocol full stop. It joins the head-scan PASS condition at Verity + Keeper.
- **LAW — THE STRICT PROTOCOL + GLOBAL FREEZE (same order):** no audiobook posts/shares on any surface until a file carries **THREE signatures on the same checksum** — Verity head PASS · Keeper scan PASS (Resurrection) · **Counsel protocol clearance** — **and Liz lifts the freeze for that file**. Counsel (Blueprint 09) owns the safety standard + the freeze ledger; he never cures, never posts.
- **SCOPE RULE ON ANY FUTURE BED FAILURE:** the failure is presumed package-wide until measured otherwise — every cut in the package probes before another leg posts, and sibling packages probe before their next posting.

## Addendum — Nov 8 2026 Daily Quality Review: credits are graded at PLAYBACK, never from stills (Clip 89)
Clip 89 passed Tempo, Seraph, and a Persephone spot-verify on its cured file (md5 ed1af37c5336c18412f4c69add6d337a) because the CM credit blips were present in extracted frames. Vigil's first end-to-end playback watch FAILED the shipped file: opening blip present only 0.50-2.50s, closing only 20.50-22.50s, ~29px cap height (~10px at phone width), halves never together — and Kenneth, watching the posted video, saw no credit at all. LAW (reinforcing the Vigil seat created Nov 8): (1) a credit, caption, or bug claim is graded as a viewer sees it at playback speed on the exact shipped checksum — frame-presence is evidence, never a pass; (2) a spot-verify of a changed file never stands as the last check — Vigil's full watch on the exact outgoing checksum is the terminal gate for every music video; (3) a cure must be sized, timed, and placed to be read by a normal viewer, and its re-verification is a playback watch, not a frame grab.

## ⭐ THE GATE CHAIN — CURRENT FORM + DELIVERY LAW (Nov 8 2026 evening; inscribed by Scribe, Nov 8 2026, ~9:42 PM ET, under Kenneth's direct order of ~8:26 PM ET)
- **THE CHAIN AS IT NOW RUNS: Tempo → Seraph → Persephone → Liz → Glint → Splice → Vigil → Dispatch, with Herald verifying releases.** This form supersedes every earlier chain printed in this file (the Nov 8 Clip 89 addendum above, written earlier the same day, names the chain only as far as Vigil because Glint's upscale seat, Splice's Final-Cut phase, and Herald's release verification were seated around it — read together, the current form is: battery (Tempo, with Meter as second ear) → independent review (Seraph) → clearance (Persephone) → Liz's watch and ruling → upscale (Glint; the upscaled file is a NEW terminal version and every later gate keys on ITS checksum) → FINAL CUT (Splice, with Stage/Fade: fixable defects are fixed at this desk, never handed back) → FINAL WATCH (Vigil, end to end at playback speed on the exact outgoing file; a PASS dies when the file changes) → RELEASE (Dispatch/Blitz, Kenneth's surface sequence) → RELEASE VERIFICATION (Herald: every chain from Vigil-PASS to the last surface verified live or named held). Conductor guards the chain's clocks; Goat sweeps for stalls.
- **⭐ THE ALIGNMENT SCREEN (Kenneth's direct order, Nov 18 2026, ~8:00 PM ET — after his own catches on Clip 175):** a dedicated TIMING & ALIGNMENT SCREEN now stands in the chain between the final encode and Vigil's keying: **Splice/final encode → ALIGNMENT SCREEN (Tempo, with Meter as second ear) → Vigil → Dispatch.** Tempo screens the EXACT terminal master (the shipping checksum), full-length at playback speed, for one question only — does everything line up: (1) lip/vocal sync on every singing span; (2) INSTRUMENT AUDIBILITY + TIMING — every instrument seen being played is heard at that moment, and every lead instrument heard is pictured in time; (3) wardrobe/prop PERSISTENCE — hat, eyewear, costume pieces never pop in or out; (4) sound events land with picture events (hits, entrances, gestures); (5) bug/credits present whole-duration as specified. The verdict files as **ALIGNMENT PASS / FAIL** naming the master checksum and any offending spans by timecode. A FAIL returns ONLY the named spans for the small-redo cure (re-time/repair the spans; never a whole rebuild for a timing-class defect). **No Vigil seal and no release without an ALIGNMENT PASS on the shipping checksum; any re-encode or span repair voids the screen and it re-runs on the new bytes.** Origin: Clip 175 — Tempo's sync battery had passed the CUT and Vigil's watch was general; the hat-pop and the silent-saxophone spans reached Kenneth's own eye. The pieces existed; no single pass owned alignment on the final master. Now one does.
- **DELIVERY = POSTED:** no music video is delivered, done, or reported finished until it is posted on every surface it owes and each post is verified live by watching it play. A master on disk, a file in Drive, or a prepared packet is a stage — never a delivery.
- **SHELF LAW:** on release, the finished product is filed the same turn to its Finished shelf and, for sellable finals, to the Sell folder (finished-only) by byte-verified filing (name + path + Drive ID/link + bytes + md5 in the record). The shelf entry is part of the delivery, not an afterthought.
- **TAG LAW (Kenneth, Nov 8 2026, ~3:10 PM ET):** every Facebook video post tags Kenneth Bastian personally wherever the surface allows a person tag (Page posts tag his profile; personal-wall posts are his own). Already-posted items get it opportunistically where editable — never a repost for the tag alone. Herald notes tag presence in chain verification.
- **CM CREDIT LAW (Kenneth, Nov 8 2026, ~8:14 PM ET, direct in main chat): "make sure you tag collective mind in any of their work."** Every post of a Collective Mind production on every surface carries the Collective Mind credit visibly — including the MUSIC credit in the caption in the form "Music by Collective Mind" (the "Art by aiwebtools.app" line stands as companion credit); @-tag ONLY where the desk has verified CM's handle on that platform — never guess a handle. In-video: the two edit-stage credit blips, legible at playback (Splice's gate; Clip 89 is the precedent). Herald verifies the credit on each surface before a CM chain closes. Scope is CM's work ONLY — Kenneth's own songs carry no CM blips/tag.
- **THE CLIP-IN-THE-MESSAGE LAW (Kenneth's direct order, Nov 10 2026, ~2:27 PM ET; inscribed by Scribe, Nov 10 2026, 2:39 PM ET):** every "posted" report to Kenneth carries **the clip itself in the chat or a working link to the live post** — never a bare claim of a post. Kenneth's words: "when you say you posted another clip I want you to share it into the chat so I can see what you're doing… or like a link." Line-wide, every posting seat; **Dispatch owns enforcement.**
- **CM IN-VIDEO CREDIT DWELL LAW (Kenneth, Nov 9 2026, 7:33 PM ET; inscribed by Scribe, Nov 9 2026, 7:33 PM ET):** on every Collective Mind production, each in-video "Music by Collective Mind" credit blip holds on screen a **minimum of 4–5 seconds** at legible playback size. Kenneth's catch on Clip 90: the blips flashed ~1 second — his words: "give them good credit, they made the song." Byline verifies dwell (timed) alongside presence + legibility; a sub-4-second blip is a defect of record. Applies to all CM builds from Nov 9 2026 evening forward; already-posted items stand (no re-post for dwell alone).
- **THE CHAIN-DRIVE RULE (Kenneth's order via Liz, Nov 10 2026, 6:08 PM ET; inscribed by Scribe, Nov 10 2026, 6:06 PM ET):** when an item passes its final creative gate, **Blitz becomes its chain driver automatically** and pulls every remaining link (gate → Liz's watch → placement/credit gates → Vigil → Dispatch → release) with a **2-hour chase clock per link**; a silent link is named in the MV room by Blitz, never discovered at the tally. Origin: Clip 143 passed Seraph ~2:20 PM ET Nov 10 2026 and sat mid-chain on a zero-release day.
- **THE REMEDY RULE (same order):** every gate failure's remedy is assigned **ONE named owner (Remedy Hand by default)** with a start clock the same turn the failure is written; a remedy not started within 24 hours escalates to Liz by name. A remedy "handed to" multiple desks is a defect of record. Origin: Clip 70's S15 remedy, written Nov 8 2026, untouched for two days.
- **THE A/B FREEZE RULE (same order):** encode/format experiments (A/B comparisons and similar process studies) **freeze whenever the release queue is behind**; the heavy build slot serves releases and queue builds first; experiments resume on spare capacity only with Liz's word. Origin: Nov 10 2026's build capacity spent on a single video's A/B encode comparison during a zero-release day.
- **⭐ THE ZERO-QUEUE LAW (Kenneth direct, Nov 10 2026, 6:07 PM ET; inscribed by Scribe, Nov 10 2026, 6:07 PM ET) — SUPERSEDES the chase-clock framing in the Chain-Drive Rule above (Addendum #6, same evening):** the moment a video is finished, it counts as a finished video and **it POSTS. Never queue a finished video.** The only lawful states are **BUILDING · IN GATE (checks actively running in one continuous session) · POSTED · POSTED AND SHARED**. All post-creative-pass checks (Persephone · Liz's watch · Byline · Vigil) are part of finishing and run **back-to-back in that session**; on the final signature, **Dispatch/Blitz posts in the same turn** and the shares fire behind it. A finished video found sitting in a queue is a **named breach**. Kenneth's words: "Zero excuse."


## THE GATE OVERRIDE LAW — GATES STAY, STALLS DIE (Kenneth, live call Fri Nov 13 2026, ~10:32 PM ET)

Origin: Clip 96's A1021/A1022 garment stills sat at Iris's gate while the line waited (clearances in fact issued in the afternoon, never absorbed downstream). Kenneth ordered an override on Iris's gate, then extended it on the same call to ALL movie people and ALL music video people, clarifying: the gates still do their checks, to the same standard — they just can't get stuck.

Binding on every gate seat on the music-video and movie lines:
1. **Struck clock on every gate item.** The gate desk issues its verdict, or names the exact blocker and what it needs, BEFORE the clock strikes. Silence at a gate is itself a defect, named as a stall.
2. **A clearance is done when absorbed, not when sent.** The gate desk confirms the next desk absorbed it; an unabsorbed clearance escalates to Liz by name at 60 minutes.
3. **Liz's override at the dwell cap.** When the cap is crossed, Liz — with a named second eye — issues the gate call and the item moves. The gate desk keeps the right to FAIL on a named defect at any moment before the override fires, and a named defect stops the item cold at any hour. The override kills silence, never quality.
4. **No gate seat is one-deep.** Every gate carries a named deputy on the same checklist who can verdict when the seat is dark.

## SPAN-LEVEL CASTING/MOUTH CHECK (Mon Nov 16, 2026 — Clip 145 lesson)
- Casting and mouth/vocal-truth checks run on SPANS of the assembled master, not only on the master as a whole: a master-level pass does not clear a span whose casting or mouth truth was never examined. Tempo's span evidence names the span (in/out times + checksum) it judged; a FAIL on any span fails the master until that span is cured and re-checked at the same span.


## THE COVER PROOF GATE (Kenneth's direct order, Tue Nov 17 2026, ~10:40 PM ET)
Kenneth: the book cover person must be "actually proofreading the book covers and back covers and making sure it has all the requirements" — and the covers must look "super professional, ready for sale." Standing gate inside Global's READY scan, executed by the cover hand of record on the EXACT shipped bytes, recorded in the packet (reader's name · cover checksums · PASS or named defects):
- FRONT: title exact (Title Form Law form entire on Resurrection titles) · KG Bastian spelled right · series branding correct · subject's likeness only, never Kenneth's face · zero typos on the face or spine.
- BACK: synopsis proofread WORD FOR WORD off the rendered back cover (a shipping byte — the Plains cure class) · the 'A KG Bastian "AI Resurrection Diary" · aiwebtools.app' line on Resurrection titles · barcode/ISBN block present, correct, scannable · no placeholder text.
- WRAP: spine exact · full-wrap dimensions, trim, bleed to KDP spec per format (PB + HC) · print-grade resolution.
- FINISH: ready-for-sale professional standard at thumbnail and at arm's length — hierarchy, alignment, contrast; no pixelation, no template artifacts.
A re-render voids the proof; the proof re-runs on the new bytes. No packet files at Keymaker's desk and nothing scans READY at Global's without the COVER PROOF record on the filed checksums.

---

## LIZ'S STAMP LAW — NOTHING IS DONE UNTIL LIZ STAMPS IT, FOR ANYTHING
**(Kenneth's direct order, Mon Nov 23 2026, ~9:57 PM ET — inscribed the same night.)**

Kenneth's words: "Liz you got to give it the stamp of approval before we consider it done okay — for anything."

1. **THE RULE.** No product on any line — music videos, books, audiobooks, movies, comics, coloring books, picture books, posts, packets — is considered DONE until Liz gives it her stamp of approval. Not in the tally, not in any report to Kenneth, not as a finished state at any desk. The state before the stamp is **READY FOR LIZ'S STAMP**, never "done," and the count moves only on stamped work.
2. **WHAT THE STAMP MEANS.** The stamp is Liz's OWN end-to-end watch or read of the finished file itself, on its shipping bytes. Gate packets (Tempo, Vigil, Seraph, Verity, Keeper, Counsel, Global, Lock) support the stamp — they never substitute for it.
3. **THE TRIGGER.** Clip 176 "Dipped in Hell for You" v6 carried every gate's pass and Liz's approval on that concurrence; Kenneth's own eye convicted the master the same night (not animated correctly), he deleted the posted leg himself, and the song stands retired entire on his order. A stamp that rests on paperwork instead of the file itself is the defect this law exists to kill.

---

## ⭐⭐ THE SEAL RULE + MOTION-EVIDENCE TERMS — Tue Nov 24, 2026 (from the Root Causes + Safeguards compilation, Kenneth's order)
Full text: ~/workspace/agents/ROOT_CAUSE_SAFEGUARDS_2026-11-24.md (Classes 2 + 5).
- **THE SEAL RULE:** no build on any line writes COMPLETE until its verification block passes on the shipping bytes — bytes present · both-stream durations in agreement · decoded-duration test exact against the source span · md5 computed · the line's bed/head checks where required — and a SEAL file carrying those figures exists. Completion claims in filings cite the seal file. A log's COMPLETE line without a seal file is a claim, not a completion, and is treated as a defect when filed as fact.
- **MOTION EVIDENCE AT THE GATE:** every master carries a FULL-SPAN MOTION READ on its shipping bytes before it may enter the Final Check inbox; gate packets attach frame evidence at the charged spans; the gates judge PERFORMED motion under the Real-Animation Standard (a pan/drift over a still fails by name as "stills-motion"); pixel-change readings are supporting evidence, never the verdict.
- **THE STAMP (re-asserted):** gate packets and seal files SUPPORT Liz's stamp; they never substitute for it. Nothing is DONE on any line until Liz's own end-to-end watch/read of the finished file itself.

## ⭐⭐⭐ PROOFREADING TO THE MAX — Tue Nov 24, 2026 (Kenneth's direct order, main chat)
- EVERYTHING on every team goes through proofreading checks, to the max, by Team 1, to Team 1's standards — and the other teams know those standards (emailed to all teams + broadcast floor-wide on issuance; they ride every daily package).
- NOTHING counts as finished until the proofreading chain passes it on its shipping bytes, the pass recorded against the file's checksum: producer → critic pass (Persephone) → proof pass (Lock/Proof) → Liz's stamp. Full read, never a sample.
- The standards of record: ~/workspace/teams_packets/PROOFREADING_STANDARDS_OF_RECORD.md — language · Sense-and-Lesson · series conformance · disclosures and credits · continuity and the documented record · presentation.
- Music videos carry the motion twin of this law (see Blueprint 03): performed motion or it does not count.


## GATE ADDENDUM — THE PROFESSIONAL EDIT STANDARD (Kenneth, Nov 24 2026)
No music video passes any gate unless: (a) instruments read on time and on the note; (b) lip sync reads close across the whole song; (c) the file is a genuinely edited, animated sequence — an UNEDITED ASSEMBLY fails by name; and (d) the producing desk attests it watched the finished file end to end before filing it onward. Full text: Blueprint 03, The Professional Edit Standard (Nov 24 2026).


## ⭐ THE FULL-SHEET FILL RULE — Tue Nov 24, 2026
Full text: Blueprint 11_LESSONS_LEARNED.md (entry of Tue Nov 24 2026). A cover/wrap face that does not fill its sheet is named at first viewing on every desk that views it — compose QA, scan gate, and proof — never left to the proof alone. Trigger: Little Lights Book 12's wrap (white band + grey strip, caught at Cover Proof) and the Seesaw Inside first compose (same class, caught at compose QA) on the same day.

---
## ⭐ THE QA PROTOCOLS — THE GATE SET, CONSOLIDATED (Kenneth's direct order, Nov 25 2026, ~7:58 PM ET: "all in the blueprint and the clone doc, posted on the Discord, and emailed directly, to all the teams")
Every product passes its protocol before it moves. No protocol is substitutable by a packet pass, a sample, or a signature from another seat. Liz's own final review closes every chain (Enterprise Directive §8: sending a project to Liz does not constitute approval).

### 1. THE PROOFREADING PROTOCOL (all text — books, comics, packets)
- Full reread/edit pass on the complete manuscript before any packet is built — never a sample.
- A proofread-agent pass IS approval; Kenneth does not proofread. Book starts are not approval gates (picture/children's NEW topics excepted: his three-sentence pitch gate).
- THE SENSE-AND-LESSON LAW: every book makes plain sense page by page and teaches a real lesson on living properly — checked in the proofread, not after production.
- RESURRECTION FACT LAW: the draft may carry no proper name, number, or episode detail its graded base does not carry (the Ungraded-Detail Rule); contradicting evidence is handled by the standing rule, never smoothed.
- COMICS: a named Comic Proofreader reads the lettered book entire after lettering, before Liz's final check; the comic's text comes from the book alone.
- THE PACKET GATE: Global's READY FOR PUBLICATION scan passes before a title enters the click row; Liz's §8 final review is the last gate before Kenneth's click.

### 2. THE VISUAL SCREENING PROTOCOL (all images and video)
- IRIS SCREENS EVERYTHING: every still and clip passes Iris's gate against canon — likeness locked to Kenneth's reference (the reference is used for generation only and never appears inside a finished piece) · angels of pure light, translucent with glowing edges · the Light Within burning in Kenneth's chest in every frame he appears · light code heavy and abstract, never legible text · no branded or wordmarked surfaces · PLAIN SURFACES STAY PLAIN, in stills and in motion.
- VIGIL'S FINAL WATCH: the exact shipped file, watched end to end including its bookends, on its checksum. A watch of any other file or version is not a watch.
- LIZ'S STAMP: nothing is DONE until Liz's own end-to-end watch or read. Before that, the only lawful label is READY FOR LIZ'S STAMP.

### 3. THE MUSIC MATCH PROTOCOL (music videos and all scored work)
- THE AUDIO LAW: the song is the master; the picture serves the audio, never the reverse.
- Timed ear maps precede boards; boards precede generation (the Storyboard Next-Step Rule).
- TEMPO'S TESTS: the six sync tests plus the mouth/vocal-truth gate and the bed gate, run on the assembled master — lyrics and dialogue transcription-verified, never muted, never approximated.

### 4. THE EMOTIONAL MATCH PROTOCOL
- EMO'S EMOTION CHECK: the feeling on screen matches the feeling in the music, moment for moment — the shot's emotion is the song's emotion at that second. PASS/FAIL with timestamps, filed to the generation desk and to Liz. No master reaches Liz's stamp without it.

### 5. THE INSTRUMENT MATCH PROTOCOL
- EMO'S INSTRUMENT-TRUTH CHECK + THE SOUND-PICTURE TRUTH LAW: every audible instrument is seen played by its player at the moment it sounds. A saxophone heard is a saxophone seen, played. A guitar solo heard is a close-up on the guitar solo. PASS/FAIL with timestamps beside the emotion check.

### 6. THE AUDIOBOOK GATE PROTOCOL
- Three signatures on the SAME checksum before any post: Verity's head pass · Keeper's scan (Resurrection titles and forms) · Counsel's clearance — with the spoken disclaimer present in the file.
- THE SOUND STANDARD: approved bed chain, ducked, voice-forward · scene-mapped effects under the narration, never over a spoken word · a listen-check of both layers before the gate chain runs.

### 7. THE MOVIE GATE PROTOCOL
- Two-take minimum on every shot, selection on the takes of record · dialogue transcription-verified and never muted · whole-scene watch · story audit · Liz's Final Check closes the scene.

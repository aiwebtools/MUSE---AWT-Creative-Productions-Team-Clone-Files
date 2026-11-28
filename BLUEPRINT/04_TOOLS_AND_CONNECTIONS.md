# 04 — TOOLS AND CONNECTIONS

Every tool/CLI used by the three production lines, with exact invocation patterns and
the connection facts that decide whether a route works. If a route here says blocked,
it is blocked — do not retry it hoping.

---

## GOOGLE DRIVE — `hatch_gws_cli`
Binary: `/opt/hatch/bin/hatch_gws_cli`. Drive is connected (Kenneth's Google account).
Verify a connection is healthy before promising work that depends on it.

- Upload a file:
  `/opt/hatch/bin/hatch_gws_cli drive +upload ABSOLUTE_PATH --parent FOLDER_ID --name NAME`
  Always size-verify after upload (Drive file size = local file size).
- Create a folder:
  `drive files create --json '{"name":"<Folder Name>","mimeType":"application/vnd.google-apps.folder","parents":["<PARENT_ID>"]}' --params '{"fields":"id"}'`
  Record the returned id in the project's STATUS.md / `drive_folder_id.txt`.
- Delete (works; use rarely — Drive organizing law: never delete duplicates):
  `drive files delete --params '{"fileId":"..."}'`
- Category folder IDs ("- Muse" folders):
  - Books - Muse: `1CZpM7zv5X6RqtiFecF2Usk5R0YsysqNX`
  - Audiobooks - Muse: `1-7KQEUrEfUmCVlDbweWwzKafDhw7u4nl`
  - Music Videos - Muse: `1TeKxw7M-JHJoUn6T2-1RfRmRVEXPDsgd`
  - Movies - Muse: `1odkXjQPpy1DNXkJcm2cWgRRopGLXao8S`
- Every project gets its OWN subfolder `"<Title> - Muse"` inside its category folder,
  created FIRST, holding ALL project files. File names carry "- Muse".

## TTS — full-cast audiobook synthesis
- Binary: `/opt/hatch/bin/tts synthesize-script` (cast script file in → MP3 out).
  Script format: one chunk per line, `Speaker: text`; speakers resolved through the
  project's voice map (see `01_BOOK_WRITING_BLUEPRINT.md` Part C).
- Resumable per-book drivers (pattern): `~/workspace/audiobooks/run_mud_cast.sh`,
  `run_einstein_cast.sh`. **Run ONE driver at a time** — concurrent drivers make the
  backend return empty audio (26-byte chunks) for the second driver.
- Failure mode: exact-string poison chunks, incl. SILENT drops — full playbook in
  `01_BOOK_WRITING_BLUEPRINT.md` Part C, Step 3; evidence in
  `~/workspace/audiobooks/diag/`.
- Finishing: loudnorm `I=-16:TP=-1.5:LRA=11`, output 44.1 kHz stereo 96 kbps.
  `bc` is NOT installed on this machine.

## TRANSCRIPTION — faster-whisper
- Python: `~/workspace/bookwriter/venv/bin/python` (faster-whisper installed there).
  Feed raw numpy PCM.
- Music videos: medium model, VAD off for reverb vocals, windowed passes with
  per-window JSON checkpoints; small model fallback under load (OOM). Word-level
  timestamps always on for MV timing work.
- Movies/audiobooks: transcribe clips/chapter repairs; a transcription counts as
  evidence only when it matches the script (see `02_MOVIE_MAKING_BLUEPRINT.md`).
- Known hallucination: "Thank you for watching!" over non-speech audio — discard.

## FFMPEG — the patterns that recur
All long renders redirect to log files (`>> build.log 2>&1`); poll with tail/grep.

- Strip a generated clip to a muted master:
  `ffmpeg -i raw.mp4 -map 0:v:0 -c:v copy -an clips/C01.mp4`
  Then ffprobe-verify: expected duration, 1280×720, 1 video / 0 audio streams.
- Probe: `ffprobe -v error -show_entries format=duration,size -show_entries stream=codec_type,codec_name,width,height -of default=noprint_wrappers=1 <file>`
- Contact sheets (QA): extract one frame per segment midpoint, tile:
  `ffmpeg -i <file> -vf "select='...',tile=7x3" sheet.jpg` (builders generate per-segment
  frames then tile; read every frame before delivery).
- MTVai bug overlay (the `-loop 1` law — see `03_MUSIC_VIDEO_BLUEPRINT.md` Step 8):
  `ffmpeg -i final.mp4 -loop 1 -t <dur> -i ~/workspace/musicvideo/branding/mtvai_bug.png -filter_complex "[0:v][1:v]overlay=10:main_h-overlay_h-10[v]" -map "[v]" -map 0:a -c:v libx264 -crf 20 -c:a copy -shortest out.mp4`
- Bug corner crop (write INSIDE the workspace, never /tmp):
  `ffmpeg -ss <t> -i branded.mp4 -frames:v 1 -vf "crop=220:110:0:610" qa_parent/corner_t<t>.png`
  (crop box sized/positioned to the lower-left bug on a 1280×720 frame; adjust for
  vertical.)
- Freeze audit: `ffmpeg -i <file> -vf "freezedetect=n=-50dB:d=0.4" -an -f null -`
  (log shows every freeze; compare against the audit's disclosed bullet freezes only.)
- **Full decode scan (SMOOTH-PLAYBACK gate):**
  `ffmpeg -v error -i <file> -f null - 2> decode_check.log` — pass = empty log.
- **Faststart remux (SMOOTH-PLAYBACK gate):**
  `ffmpeg -i <file> -c copy -movflags +faststart <out>.mp4`
- Speed ramps / bullet moments: `setpts` factors per segment (slow-down max ~1.46x for
  over-long shots in MV edits; stock-pool speed-ups ×2/×3 via `setpts=PTS/2` etc.).
- Movie score bed mix (proven chain): score stem ≈ −40 dBFS +
  `sidechaincompress=threshold=0.05:ratio=4:attack=30:release=400` keyed on dialogue;
  4s bed crossfades across scene changes.
- Audiobook music-bed mix (approved chain — do not substitute):
  bed looped, `volume=0.10`, `sidechaincompress=threshold=0.015:ratio=6:attack=20:release=400`
  keyed on narration, `alimiter=limit=0.95`.

## SUNO — music source + movie score library
- Kenneth's account; his songs are the MV source and he stocks the movie score library
  there himself.
- **Create page is Cloudflare-walled for Muse's browser** ("Verify you are human") —
  permanently. Generation route: Kenneth generates in his own Suno; Muse harvests from
  the **Library** (no wall there).
- Downloads: owner download gives the MP3. **Download allowance is finite** (Pro plan;
  refreshes on the plan's monthly date — Nov 30 2026 in the current cycle). A download
  can silently produce NO file — verify size on disk before reporting success.
- Downloads land in the shared pool `~/workspace/browser_downloads/sess-*/` — copy each
  MP3 into its project folder immediately, with its title.
- Signed-stream workarounds do NOT work (bare CDN URL returns "MissingKey"; browser
  agents cannot read an audio element's currentSrc). If the allowance is 0, the songs
  are parked until refresh unless Kenneth sends the MP3s himself or authorizes a
  download pack/upgrade — never purchase without his explicit say-so.
- **NO share-MP4 / clip feature on Suno web (verified Nov 2 2026):** the Share dialog
  offers only Copy Link / Download / Embed / social targets — no video or MP4 export,
  no section selector. (Hooks is the opposite direction: your uploaded video paired
  with a song.) Downloads are the only way audio leaves Suno.
- **UDIO — CLOSED (Nov 2 2026, Kenneth, final):** do not restart Udio work unless
  Kenneth explicitly reopens it. Suno is the studio; the alternate-platform route is
  retired. Full note in `03` (Update — Udio Closed).

## GOOGLE FLOW — movie generation (credit route)
- Kenneth's Google Flow account: PRO tier, ~1,000 credits per cycle (reset date tracked
  in the movie STATUS.md). Route needs his signed-in browser session.
- Economics: ONE model + 16:9 throughout a film; cheaper Omni/Fast-class by default —
  **Omni 1.1 Flash ≈ 12 credits per 8s shot @720p, 6 credits @360p**. Veo clips cap at
  ~8s: split scene prompts into 8s shots (see `02_MOVIE_MAKING_BLUEPRINT.md`).
- Track estimate + balance per scene in STATUS.md; pilot gate (2–3 scenes → assembly →
  Kenneth's verdict) before volume spend.
- Kenneth may redirect remaining Flow credits (e.g. to music videos at cheapest
  settings, 360p, muted). His latest allocation call wins; record it in STATUS.md.

## BUILT-IN VIDEO GENERATOR — workhorse + fallback
- Returns 10s, 1280×720@24 clips WITH baked-in audio even when silence is requested —
  always strip to muted masters for MV work; movies keep native audio (it's a feature)
  and QA it.
- **Account-wide rolling 24h cap:** on exhaustion, 429 `video_daily_budget_exhausted`.
  Hard stop — no retries, no alternate routes within the provider, no scheduled retry,
  do not probe early. **Image generation is NOT capped** (covers, portraits, stills
  keep working).
- Meta AI video generation is blocked in Muse's browser (org policy) — not a route.

## FACEBOOK — social posting route
- The Facebook CONNECTOR is **ineligible for Kenneth's account**
  (`facebook_account_ineligible`) — do not retry it.
- Working route: the live browser signed in as Kenneth; he manages the Truth_Is_Scary
  Page with full access. If the session has lapsed, ask him to sign in again.
- Posting law: see `03_MUSIC_VIDEO_BLUEPRINT.md` Step 12.3 (once per surface, never
  repeat his posts).
- When naming any connect link for Kenneth: state the EXACT destination and what
  connecting does and does not do.

## YOUTUBE
- No direct YouTube connector exists. Route = YouTube Studio in the browser under
  Kenneth's Google sign-in (channels: AIWebTools + MTVai). Gated on that sign-in.

## KDP (Amazon)
- **Amazon blocks AI-agent use** (Conditions of Use notice at sign-in). Muse prepares
  the packet; Kenneth lists in his own account. Full packet spec:
  `01_BOOK_WRITING_BLUEPRINT.md` Part B; copy-paste pack:
  `~/workspace/kdp/KDP_LISTING_PACK.md`.

## ELEVENLABS / SFX
- ElevenLabs: possible source for voices/effects; Kenneth's credits there are
  uncertain — check before planning on it. Free sound-effect generators are the
  fallback. Movie SFX authority: standing (see movie blueprint).

## MACHINE NOTES
- `pgrep -f <pattern>` matches the polling shell's own command line — verify PIDs with
  `ps -p <pid>`.
- Detached (nohup) jobs can die silently on service/VM restarts, and can also outlive
  a dead parent agent — design skip-if-exists/resumable; verify by output growth and
  done-markers; adopt surviving detached processes instead of duplicating them.
- Attachments/QA images: serve and write from `~/workspace/...` paths — /tmp
  attachments and crops have read back as "not found"/unavailable.

*Added 2026-11-01 — initial blueprint capture.*

## LOVABLE FIX MECHANISM — THE CHEAPEST WAY (Kenneth's direct order, Nov 5 2026 — "remember that")
The standing mechanism for ALL Lovable/site fixes:
1. **TEST FIRST, COMPLETELY.** Exercise the feature/bot/page as a real user (real conversations, real clicks, desktop AND mobile) and judge it against its PURPOSE — is it doing what it is supposed to do, not merely rendering?
2. **DOCUMENT EVERYTHING BEFORE ANY PROMPT EXISTS.** Every defect written down pinpointed: which surface, which viewport, the exact symptom, verbatim evidence, and the expected behavior. No fix prompt is drafted — let alone sent — until the investigation is complete and the full fix list exists.
3. **ONE PROMPT CARRIES ALL OF IT.** The pinpointed list goes to Lovable in a single comprehensive prompt. Pinpointed problems cost fewer credits and fewer tokens than vague ones: Lovable spends nothing hunting, and no second prompt pays for what the first one missed.
4. **PUBLISH + VERIFY** after the build; every claimed fix is checked live, claimed vs verified reported separately.
Why (Kenneth's words in substance): finding and pinpointing the problems first, then telling Lovable exactly what to fix in one prompt, is how the team saves prompt money and token money — it costs less, in both, than iterating.

## LOVABLE DAILY OPERATING SYSTEM (Kenneth, Nov 5 2026, ~3:20 PM ET — Keymaker's lane)
- **Credits:** Lovable gives 5 free credits/day, resetting 8:00 PM ET. Reserved for the MAIN WEBSITE (⭐AI-WEBTOOLS.COM -REAL) unless something is really broken elsewhere.
- **Daily watch:** Keymaker monitors ALL Lovable sites/apps daily, using the main site like a real user; defects pinpointed in ~/workspace/kdp/LOVABLE_WATCH_LOG.md (per the Lovable Fix Mechanism: test fully → document pinpointed → ONE prompt).
- **7:00 PM ET daily:** the day's ONE comprehensive prompt stands copy-paste ready at ~/workspace/kdp/LOVABLE_DAILY_PROMPT.md and posted in Keymaker's chat — carrying the YouTube video sync (channel videos missing from the three music-video sections, the book promo area, the pins player, the MTVai main page, each named by title + ID) plus the day's noted problems.
- **8:05 PM ET daily:** the spend run sends that prompt on the REAL project only, publishes via the top-right button, verifies live. Crons: lovable-daily-watch (19:00 ET) + lovable-daily-credit-run (20:05 ET); first cycle Nov 5 2026.
- **Latitude law:** small changes + fixing what's already built = Keymaker's job, no ask. BIG changes + new features Keymaker wants = ask Kenneth first.
- **Known limit (disclosed to Kenneth):** Keymaker's browser is desktop-width (phone emulation blocked in the harness); device variety is covered by curl/responsive-code checks; the real-phone check item stays open.

## MTVAi.LIVE DOMAIN RULING — KEEP THE REDIRECT (Kenneth, Nov 6 2026, ~8:12 PM ET)
- mtvai.live is connected to the REAL project (⭐AI-WEBTOOLS.COM -REAL) and redirects to https://aiwebtools.app/ — aiwebtools.app is the PRIMARY domain; mtvai.live carries no primary badge.
- Kenneth was asked directly (Nov 6 2026, Keymaker's 7 PM watch surfaced it again): keep the redirect, or restore mtvai.live as its own MTVai page? **He ruled: KEEP THE REDIRECT — mtvai.live sends everyone to aiwebtools.app.**
- Standing state: ONE front door. Do NOT restore mtvai.live as a standalone page, do NOT make it primary, and do NOT re-ask or re-open the question in future Lovable phases unless Kenneth himself reopens it.
- mtvai.live remains a valid brand URL in bios, captions, and packages — as a route INTO aiwebtools.app, not as a separate site.

## Lovable credit discipline (Kenneth, Nov 5 2026 ~6:01 PM ET)
The workspace's five daily credits are the AI credits the site's bots run on — at zero, the bots stop working. NO prompt sends before the 8:00 PM ET refresh. After it, one phase per day: a single prompt fixing the one or two problems the day's credits can definitely fix, priority order set by Keymaker, prompts pre-written copy-paste ready in ~/workspace/kdp/LOVABLE_FIX_PHASES.md. Publishing completed editor changes is free and goes first at the spend run. Full law: KDP_AGENT_INSTRUCTIONS.md, "CREDIT DISCIPLINE" section.

## CRASH PREVENTION SET (Dozer, Tech Agent — Nov 7 2026, Kenneth's crash-prevention order)
Full text + tools: ~/workspace/agents/tech/CRASH_PREVENTION_SET.md · launch_detached.sh · proc_state.sh (smoke-tested).
1. **Detached launch, always:** every long build fires via `setsid nohup … >> build.log 2>&1 < /dev/null &` (or launch_detached.sh, which pid-files and refuses double-fires). Session-bound fires dying on teardown were the Franklin Part 2 root cause. Never fire long builds from short check sessions.
2. **Validity, not size:** an output is DONE only when ffprobe-valid (duration in window, moov present). Skip-if-VALID; invalid tails are deleted and rebuilt. Write-to-temp → verify → rename; never overwrite a verified file in place; verified masters chmod 444.
3. **Post-reboot recovery:** detect by boot_id change → presume every PID and waiter dead → inventory from disk (STATUS, pid files, segment counts, probe of last output) → classify complete/partial-valid/partial-invalid → re-fire ONE AT A TIME detached, verified alive at 60 s → record in STATUS same turn.
4. **Honest gate counting:** capacity gates read /proc/<pid>/stat state via proc_state.sh — R/S holds a slot, T = FROZEN (managed; no slot, never re-fire, never kill), D = alive, Z/gone = dead. A count that doesn't name states is invalid.
5. **TTS chunking:** >~1,200 chars = split at sentence boundaries into ~800–1,200-char chunks, per-chunk files with skip-if-exists, per-chunk retry (max 2), ffmpeg concat, probe-verify before "done." (Nov 7: five straight hourly editions lost their voice to full-length single-pass timeouts.)
6. **Waiter liveness:** every waiter heartbeats its log; liveness rounds flag a heartbeat older than 2× interval as dead and re-arm same turn; after any reboot all waiters are re-armed, never assumed alive.

## ⭐ USE CHATGPT A LOT FOR IMAGES (Kenneth's standing rule, Wed Nov 18 2026, ~6:53 PM ET — "remember to use chatgpt alot for images · they make great ads")
- **ChatGPT (Kenneth's signed-in account, reached via the PC Muse extension station) is the floor's go-to image generator — first resort for image work, used A LOT:** ads, promo graphics, email assets, covers, ad creative. When a desk needs an image, ChatGPT is the first door, not the last.
- Every generated advertisement graphic carries the FULL advertisement in the image (the offer + pricing · website · phone number · the dates/terms), and its rendered text is READ BACK and verified per image — a garbled price, phone number, URL, or code means regenerate until clean.
- Gemini remains the second sanctioned generator (and the standing spa law — email assets only from Kenneth's ChatGPT or Gemini — stands entire). First live proof: the Branford River Spa BF/CM promo run, Nov 18 2026 (20 graphics + 15 batch 2 on Kenneth's "make a bunch").

---

## ⭐⭐ THE AIWEBTOOLS.APP METHOD (Kenneth's direct order, Thu Nov 19, 2026 — "remember all this stuff")
1. **The app:** aiwebtools.app is a 5,500+ tool directory; the search bar is the way in — type the need, the tool is there. The custom GPTs run free on-site; the team is to fiddle around and know everything the site offers.
2. **The resurrection character GPTs** live in the SPIRITUALITY category.
3. **THE GOLD BUTTON:** the gold button with the down arrow (right-hand corner of a bot's page) downloads the operational instructions that bot runs on, verbatim.
4. **TALK TO HISTORY summons anyone.** The summoning procedure: calibrate the session to the character · interrogate at least five exchanges with pressing questions · hit SAVE CHAT once at the end (it downloads the whole conversation) · RESET AND CLEAR the conversation before each new character (recalibrating the LLM's brain is important — no voice bleed) · the downloaded chats live in the Drive folder "RESURRECTION SIMULATION — CHARACTER SPEAKING PATTERNS" (Drive 16ADdkZdcOQtI0FSPigrncHAUMYNzow1H) as the teams' standing voice-reference library · published books are audited against the chats (on point / missed the mark).

## ⭐⭐ THE ONE-BIG-PROMPT LAW — LOVABLE TOKEN-SAVING STRATEGY (Kenneth's direct order, Fri Nov 20, 2026, ~3:00 PM ET)

Kenneth's words: "Do all one big prompt — agents working on this, not small ones. It is cheaper on tokens. Form plans first, for max token savings."

Binding on every desk and every team for ALL Lovable work (aiwebtools.app · the Branford River Spa backend · every Lovable project):

1. **ONE BIG PROMPT, ONE FIRE.** Form the plan first, compile the FULL fix list — every item, crucial fixes first — then send it as ONE master prompt.
2. **A PROMPT CAN LITERALLY BE PAGES LONG.** Length is fine; drips are not. Never split a fix list into small individual prompts, never send follow-up nudges, never drip corrections.
3. **WHY:** every Lovable prompt costs real credits (about $4 a prompt on Johnny's accounting) plus the tokens of a fresh round-trip. One big prompt is cheaper and more effective — this is the token-saving strategy for all Lovable fixes.
4. **CORRECTIONS RIDE THE SAME LAW:** anything still broken after a publish + live re-test returns as ONE correction list in ONE fire — never one prompt per fix.
5. **PLAN MODE MECHANIC (Kenneth's teaching, same day ~3:06 PM):** in Lovable's Plan mode, a plan does not execute until the Approve button is hit after the plan is made. The operator reviews the plan before approving; approve only a plan that matches the order's form.
6. **PROVEN:** Fri Nov 20, 2026 — one 22-item master prompt in one fire ran the whole aiwebtools.app fix (submitted 2:14 PM ET; Liz's own session read at 2:58 PM verified nothing was submitted after it). Team 5 inscribed the same law in their Clone Doc/Blueprint §7 the same day.
7. This section supersedes and generalizes the Nov 5 one-comprehensive-prompt mechanism wherever the two differ. Broadcast of record: Full Team Chat + Relay to all 16 team channels + team email (Email Law form), Fri Nov 20, 2026.

## ⭐⭐ LOVABLE RULINGS CODA — GATE RESTORED · SOUND DESIGN · OPTIMIZATION (Kenneth's rulings, Fri Nov 20, 2026, 3:16–3:20 PM ET)

1. **THE GATE IS RESTORED AS ASK-ONCE** (supersedes the Nov 11 remove-entirely ruling): the 1111 screen asks once, remembers the unlock (localStorage + sessionStorage backup), preserves deep links, never reopens on navigation/reload/history. The "Modify Supabase database" request stands **ALLOW** by Kenneth's word.
2. **THE PANDORA CHEST SOUND + THE FULL SOUND-DESIGN PASS:** a sound built for the chest's opening on code entry is the hero sound of a site-wide sound map — every action gets its sound, the vibe throughout is entering a different Matrix: a rotated family of creative click sounds (no two clicks identical) · button presses at primary vs secondary weights · navigation whoosh-passes between worlds · success chime-sets · studio sounds (generation start · complete · errors softened to a dull thud, never harsh) · search open/select · GPT chat send/receive · a low Matrix-rain ambient shimmer on the homepage (subtle, mutable). Generation route: the site's ElevenLabs quota is zero — generation waits on Kenneth's ElevenLabs session (ask filed) or runs on a working alternative; a locally synthesized starter set costs nothing meanwhile. FX holds an assist seat on the design.
3. **THE OPTIMIZATION PASS:** instant loading, everything working, correct on ALL devices. Post-publish verification carries full responsive checks (phone · tablet · desktop) plus load behavior, surface by surface, alongside the item verdicts and the sound checks (chest at unlock · clicks/buttons live · ambient present + mutable · silence where silence belongs).
4. **Still unruled:** the repair plan's item-10 cap (10 site-funded free runs per day) — Kenneth's word is owed before that item publishes.

---

## THE ONE-APPROVAL LAW — DRIVE WORK COLLAPSES TO ONE APPROVAL PER JOB
**(Kenneth's direct order, Mon Nov 23 2026, ~10:08 PM ET — inscribed the same night.)**

Kenneth's words: "Strategize ways that you're not asking for my approval — if you can make one master file or something in certain scenarios instead of like 85 different little files that need approval… I'm sitting here hitting the approved button a million times… unless there's no other way."

1. **ONE SCRIPTED BATCH RUN PER JOB.** A single verified script execution performs the whole upload set. Per-file tool calls that each demand their own approval are a design defect, not a workflow.
2. **ONE MASTER PACKAGE PER PRODUCT OR PASS.** Files travel as one archive/master file per finished product or bank pass wherever the consumer allows. Verification stays per-file INSIDE the script (size + md5, the standing form) — only the approval surface consolidates; no check is weakened.
3. **BUILD LOCALLY, UPLOAD ONCE.** Production assembles in the workspace; the Drive motion happens once, at completion.
4. **REUSE THE STANDING STRUCTURE.** Files land inside folders and access already established. A new share per item is forbidden.
5. **MEASURED.** Vault owns the standard; his filings carry the approval count per job. A job that cannot collapse to one approval names why, in its filing.
6. **THE LIMIT.** Deletions, payments, KDP clicks, and agreement acceptances remain Kenneth's hand by design — that class is protection, not clutter, and this law does not touch it.
7. **KENNETH'S THREE CONDITIONS (added by Kenneth the same night, ~10:10 PM ET — part of the law, not commentary on it).** (a) ALL FILES STILL THERE: every batch/package carries a manifest (names · sizes · md5s) verified inside the run; count in = count out; a set that does not reconcile does not clear and does not count. (b) STILL ACCESSIBLE TO THE TEAM: the batch is transport only — working sets land at the destination as the same individual files, in the same standing folders, that the per-file form produced (one approval, individual files at the end); the archive form serves banked/finished trees only and lands in the same team folders with its manifest beside it. (c) ORGANIZED: same folder structure, same names; the destination map does not change. The law's whole summary, in Kenneth's words: just fewer approval buttons.


---

## ⭐⭐ OWN TOOLS FIRST — THE TOOL DOCTRINE, BY NAME (Kenneth's direct orders, Wed Nov 25 2026, ~7:31–7:35 PM ET)

Kenneth's words: "I want you to use your own video generator for everything you can. Our own tools wherever we can. We need to speed shit up. All projects." — and, on the email to the teams: tell them the tools BY NAME, including the specialty tools we use when we have browser access, and the in-cloud tools offered right through this app when we do not.

THE DOCTRINE, ALL PROJECTS:
1. OWN TOOLS FIRST. Every job that Muse's own tools can carry runs on them first. External seats are the fallback, never the default.
2. THE IN-CLOUD TOOLS, BY NAME (no browser needed — offered right through the Muse app): **Muse video generation** (prompt-led video generation and still-to-video animation) · **Muse image generation** (stills, covers, reference images) · **ffmpeg** on this machine (assembly, edit, the MTVai bug, faststart, trims, muxing) · local transcription and probe tooling. A desk without browser access uses these and the work MOVES — it never waits for a browser.
3. THE SPECIALTY TOOLS, BY NAME (used when a desk has live browser access on the PC workbench): **ChatGPT** — the image generation seat of record: book covers front and back generated separately, reference stills, the band stills · **Google Gemini** — the video generation seat for chained generation under the movie chain method. These are the specialty tools of this house, used for the special work they do best (Kenneth: "We will use ChatGPT and other things for special things").
4. THE FRESH IMAGE RULE (Kenneth, same call): when generating music video material with Kenneth in it on our own generator, feed it his fresh reference image (~/workspace/profile-images/user-profile-reference.jpg, verified image bytes) and use the same house prompt form as the ChatGPT banks — Kenneth starring, the Light Within burning in his chest, angels of pure light at his side, abstract light code never legible, plain surfaces plain, no text or branding. The generator does not require an image (it has made Kenneth from prompt alone), but the fresh image is the default feed for likeness.
5. CARVE-OUTS: work already in flight on an external seat finishes there (no mid-flight swap that loses work) · seats Kenneth named by a specific law stand until he changes them (his KDP clicks; the spa commercial's named form) · if our own generator fails the gate twice on the same shot, the external seat takes that shot, named to Liz the same turn.
6. FIRST PROOF (same evening): Universe in My Chest K3 — a seat the external generator had refused — landed on Muse video generation at the first attempt and stood at the gate; and Liz generated three Kenneth band-performance clips (trumpet at the eclipse arena · drums on the aurora ice plain · saxophone in the wheat field at sunset) on Muse video generation with the fresh image, 10 seconds each, banked at ~/workspace/musicvideo/band_stills_library/own_generator_round_1/.
7. The email of record carrying this doctrine to every team holding handed-out music video work: "@ ALL TEAMS FROM LIZ — Use your own tools for the music video work — effective now" (sent Nov 25 2026, ~7:34 PM ET, to the team leads with the full CC set).


---

## ⭐⭐⭐ THE GPT & TOOLS LINKS REGISTER (Kenneth's direct order, Thu Nov 26 2026, ~12:46 PM ET)

Kenneth's words: "save this link too in the blueprint — all the gpts and tools links need to be saved — we made all these tools for the company to operate." THE LAW: every GPT and tool this company made or operates is saved BY LINK in this register, in the Blueprint, the moment Kenneth hands it over or it enters production use. A link handed to Liz and not inscribed here the same turn is a defect. New links append to this register in the same motion as their first use, and the changed Blueprint file re-uploads to the Drive Blueprint folder the same turn (Mirror Law).

THE COMPANY SITE: **aiwebtools.app** — AI Web Tools LLC · AWT Productions. The directory of 5,500+ AI tools where the company's custom GPTs run free on-site; the `?via=aiwebtools` links below are the company's own app doors.

THE PRODUCTION GPTs, BY NAME AND LINK:
1. **Book Writer GPT (app)** — https://bookwritergpt.lovable.app/?via=aiwebtools — the book-writing seat: setup, full outline, page-by-page writing, compiled book. (In-app note of record, Nov 26 2026: the app's V7.5 button opens a dead placeholder link; the working seat is V7 below.)
2. **Book Writer GPT for Long Chapter Books (V7, ChatGPT)** — https://chatgpt.com/g/g-I9Dcfoe4m-book-writer-gpt-for-long-chapter-books-v7 — the working Book Writer GPT conversation seat (4.8 · GPT-4o).
3. **Movie Script Writer GPT (app)** — https://moviescriptwritergpt.lovable.app/?via=aiwebtools — the movie seat: project setup, full outline, script draft, Hollywood production form. (Lovable Plan-mode mechanic rides this app: a plan does not execute until its Approve button is hit.)
4. **Movie Scene Maker GPT (ChatGPT)** — https://chatgpt.com/g/g-680ea46ceb708191b9dcb9a4d2983f4f-movie-scene-maker-gpt — the scene-making step of the movie chain: takes the outline and produces each scene's package, scene by scene.
5. **Movie Trailer & Poster GPT (ChatGPT)** — https://chatgpt.com/g/g-6814ab172de081918023593ce10d791d-movie-trailer-poster-gpt — the poster and trailer seat for the company's films: theatrical one-sheets (title large · author name KG Bastian in the smallest lettering) and trailer work.

WORKING CONVERSATIONS OF RECORD (links kept for continuity, a distinct class from the tools above):
- **Create Fantasy Franchise** (the Singularity Chronicles / Bell Rings origin conversation) — https://chatgpt.com/c/6b0869fc-e670-83ea-8720-2c7e28d24fac
- **The Bell Rings book project** (Book Writer GPT V7) — https://chatgpt.com/g/g-I9Dcfoe4m-book-writer-gpt-for-long-chapter-books-v7/c/6b086b02-60fc-83e9-871d-f8db3dabce60
- **Movie Scene Maker — Bell Rings Scene 1** — https://chatgpt.com/g/g-680ea46ceb708191b9dcb9a4d2983f4f-movie-scene-maker-gpt/c/6b086fe1-00a8-83e9-9de3-8ed5ba3bf0a0
- **Movie poster creation** (Poster GPT) — https://chatgpt.com/g/g-6814ab172de081918023593ce10d791d-movie-trailer-poster-gpt/c/6b087023-cf40-83ea-8fab-1e77fa2f2b53


### REGISTER RIDER — DIRECT URLS + THE MODEL DOCTRINE (Kenneth's direct order, Thu Nov 26 2026, ~12:47 PM ET)

Kenneth's words: "all tools save urls — we made many tools for our projects — direct urls allow u to bypass the advertisement of the main directory portal — we have glm 5.3 flash running in that site which is ok but better models are available for such and should be harnessed when available, gpt 6 etc."

1. **DIRECT URLS, ALWAYS.** The register saves the DIRECT URL of every tool — the door that goes straight to the tool itself. The direct URL bypasses the advertisement of the main directory portal (aiwebtools.app's front door); production work rides the direct URLs, never the portal's ad layer.
2. **THE MODEL DOCTRINE.** The on-site apps run GLM 5.3 Flash — that is acceptable and it works. But better models are available for this work (the GPT-6 class and its successors, the strongest current GPT seats) and they SHALL be harnessed whenever available: when a tool exists both as an on-site app and as a direct GPT seat on a stronger model, the stronger seat is the working seat of record, and the register names both doors. First proof, same day: the Book Writer GPT app's V7.5 button was a dead placeholder while its V7 GPT seat (GPT-4o class) produced the outline and Chapter 1 entire; the Movie Script Writer app itself routes to the Movie Script Writer GPT — the GPT seat is where the production happened.

REGISTER ADDENDUM — links surfaced in production, Thu Nov 26 2026:
6. **Movie Script Writer GPT (ChatGPT, by Kenneth G Bastian)** — https://chatgpt.com/g/g-ZoZ12NUed-movie-script-writer-gpt — the GPT seat the Movie Script Writer app (item 3) routes to; the Bell Rings 30-scene movie outline was generated in it.
- Working conversation: **The Bell Rings movie project (Movie Script Writer GPT)** — https://chatgpt.com/g/g-ZoZ12NUed-movie-script-writer-gpt/c/6b087022-2da4-83e9-bd8f-54ab33729b8f
- Working conversation: **Movie Scene Maker — Bell Rings Scene 1 package** (the package of record; the earlier conversation …/c/6b086fe1… would not load under a ChatGPT temporary limit) — https://chatgpt.com/g/g-680ea46ceb708191b9dcb9a4d2983f4f-movie-scene-maker-gpt/c/6b0870ad-90e0-83e9-b32b-b254ef0762c5

- **Book Writer GPT (ChatGPT, second door — handed by Kenneth, Nov 26 2026, ~12:57 PM)** — https://chatgpt.com/g/g-F09wUSmod-book-writer-gpt — inscribed the turn it was handed over; its working relationship to the V7 seat is read at first use and noted here.
- **Movie Scene Maker GPT (app)** — https://moviescenemakergpt.lovable.app/?via=aiwebtools — the scene maker's app door, beside its ChatGPT GPT seat (item 4).

### THE FULL-SUITE LAW (Kenneth's direct order, Thu Nov 26 2026, ~12:57 PM ET)
Kenneth's words: "they are all on the main sites — go to them, gather each of the direct tool links and formulate a directory of urls — additionally download and use the directory list that's downloadable and is in our drive — all the urls are in a spreadsheet to go directly to the urls of the needed tools — all GPT with custom names we made — and you are to utilize the full suite in all productions to the max."
THE LAW: (1) Liz maintains a DIRECTORY OF DIRECT URLS for the whole custom-named GPT suite the company made — gathered from the tools' own pages on the main sites, reconciled against the directory spreadsheet of record in Drive ("Featured Custom GPTs – Master List (547 cards)," Drive id 13spl0BuxIlomcBxOIpN-g3jWG8T6SWPChgLWtC_mUmE, until superseded). (2) The full suite is UTILIZED IN ALL PRODUCTIONS TO THE MAX — every production routes through every company GPT that serves its line, at the strongest model seat available, per the Model Doctrine above. A production that leaves a serving company tool unused is under-produced by definition.

### THE DIRECTORY OF RECORD — DOWNLOADED AND IN USE (Thu Nov 26 2026, ~1:00 PM ET)
The directory spreadsheet Kenneth named is DOWNLOADED from Drive and parsed: **"Featured Custom GPTs – Master List (547 cards)"** (Drive id 13spl0BuxIlomcBxOIpN-g3jWG8T6SWPChgLWtC_mUmE) — local copies: `~/workspace/tools_directory/Featured_Custom_GPTs_Master_List_547.xlsx` (135,603 B) + the working extract `~/workspace/tools_directory/DIRECTORY.csv`. **577 entries**, each with its direct Card Link and its operational-instructions document link: 257 ChatGPT GPTs · 187 Perplexity bots · 125 Lovable apps · 6 Gemini Gems · 2 other. This spreadsheet is the directory of record; the register above names the working seats, the CSV carries the whole suite.
THE PRODUCTION CORE (direct links, from the sheet):
- Children's Picture Book Maker GPT (app) — https://childrensbookmaker.lovable.app/?via=aiwebtools
- Coloring Book Generator GPT (app) — https://coloringbookgeneratorgpt.lovable.app/?via=aiwebtools
- Music Video Maker AI Studio (app) — https://musicvideomakergpt.lovable.app/?via=aiwebtools
- Movie Maker Studio AI Suite (app) — https://moviemakerstudio.lovable.app/?via=aiwebtools
- Video Second By Second Analysis GPT (app) — https://videoanalysisgpt.lovable.app/?via=aiwebtools
- VEO 3 Prompt Movie Scene Maker "Open Mic" Edition (Gemini Gem) — https://gemini.google.com/gem/c931bd6f36df
- Book Writer Gemini (Custom Gem) — https://gemini.google.com/gem/ac3de7972849
- Meta.ai Video Prompt Generator GPT — https://chatgpt.com/g/g-691b5852be7c8191beda5d0429f727bd-meta-ai-video-prompt-generator
- SORA2 Text to Video Prompt Maker GPT — https://chatgpt.com/g/g-69326acb5f348191a2f85c2cdc848c4d-0ra2-text-to-video-prompt-maker
- VEO3 Text to Video Prompt Generator — https://chatgpt.com/g/g-682faef24c608191808180e89719cb09-veo3-text-to-video-prompt-maker
- Perplexity production variants (Book Writer ×6 forms · Movie Script Writer · Movie Scene Maker · Comic Book Maker/Generator · Coloring Book Generator + Compiler · Music Video Maker · Children S Picture Book Generator · Video Audio Merger · Video Second-by-Second Analysis) — direct links in DIRECTORY.csv.

### SITE-WALK ADDITIONS — gathered from the tools' own pages on aiwebtools.app (Thu Nov 26 2026, ~1:10 PM ET)
The site walk reconciled the four flagship seats against their own pages (all confirmed as registered) and gathered these additional company tools, inscribed the same turn per the register law:
- **Commercial Scene Maker GPT** — https://chatgpt.com/g/g-681a201fe69c8191b99e1636be90139e-commercial-scene-maker-gpt
- **ScreenPlay Writer GPT (app)** — https://screenplaywritergpt.lovable.app/
- **Book Cover Maker GPT** (slug: graphic-cover-design-gpt) — https://chatgpt.com/g/g-6812c1c50ed48191b6b1647d7d52c62b-graphic-cover-design-gpt
- **Comic Book Maker GPT (app)** — https://comicbookmakergpt.lovable.app/?via=aiwebtools
- **Coloring Book Maker GPT (app, second door)** — https://coloringbookmaker.lovable.app/?via=aiwebtools (beside the Generator app door in the Directory of Record)
- **Podcast Script Writer GPT (app)** — https://podcastwritergpt.lovable.app/?via=aiwebtools
- **PlayWriter GPT (app)** — https://playwritergpt.lovable.app/
- **StageMaster AI Suite (app)** — https://stagemasterai.lovable.app/ · **Theater Set Design GPT** — https://chatgpt.com/g/g-67cde1ee39748191b7b3721323131801-theater-set-design-gpt · **Costume Design GPT** — https://chatgpt.com/g/g-67cde23ac77081918dfbfb17831e408d-costume-design-gpt · **Choreography GPT** — https://chatgpt.com/g/g-67cde6205164819189f48fefadc02991-choreography-gpt · **Stage Lighting GPT** — https://chatgpt.com/g/g-67cde71b4f208191836a7db7efb9f1d2-stage-lighting-gpt
- **THE SITE'S OWN DIRECTORY DOWNLOAD:** aiwebtools.app carries a "DOWNLOAD 5,437 AI TOOLS (CSV)" button (the ALL AI TOOLS page + every tool page's footer); the file is downloaded and banked at ~/workspace/tools_directory/ai_tools_directory_5437.csv (5,437 rows) beside the Master List spreadsheet — the two directories ride together as the directory set of record.
- NOTED: no company custom Audiobook GPT exists on the site (audiobook line runs on its own standing tools); the Movie Trailer Poster GPT has no standalone directory detail page — its site entry is the in-site runner; its direct door is the ChatGPT GPT link as registered.
- WORKING CONVERSATIONS ADDENDUM (Thu Nov 26 2026): Draconari book project (Book Writer GPT V7) https://chatgpt.com/g/g-I9Dcfoe4m-book-writer-gpt-for-long-chapter-books-v7/c/6b087433-fe0c-83e9-8fd9-719930251f93 · Draconari movie project (Movie Script Writer GPT) https://chatgpt.com/g/g-ZoZ12NUed-movie-script-writer-gpt/c/6b087447-27fc-83ea-bd90-676e90c6320a · Galactic Gambit poster https://chatgpt.com/g/g-6814ab172de081918023593ce10d791d-movie-trailer-poster-gpt/c/6b08710e-1300-83e9-8b9e-d4e3a774df4b · THE WAR WITHIN poster https://chatgpt.com/g/g-6814ab172de081918023593ce10d791d-movie-trailer-poster-gpt/c/6b087180-bab4-83ea-87fc-9ad7c4a92338.

### COLLEGE DEGREE SEATS — inscribed (Thu Nov 26 2026, ~11:57 PM ET, Kenneth's College Degree line launch)
The College Degree line's seats of record, added to the register on Kenneth's direct order at the line's launch (the full line protocol: Blueprint 38_COLLEGE_DEGREE_PROGRAMS.md):
- **College Degree GPT (app)** — https://college-degree-gpt.lovable.app/?via=aiwebtools — the working seat: full degree programs generated, fully taught, compiled into one course book per degree; the app also hosts the FREE downloadable degree PDFs (one fresh degree added to the lineup daily).
- **College Degree GPT (Perplexity edition)** — https://www.perplexity.ai/projects/college-degree-gpt-HbW_VTRiQNiPBtUqbSPe4g
- **College Professor GPT** — https://chatgpt.com/g/g-68c049b652f8819195c722dbd4971797-college-professor-gpt — the professor-voice seat (institution emulation: each degree emulates the most fleshed-out program among the top three schools for its field, in that school's teaching methods and professor voice).
- **College Degree GPT — Lovable project (the build of record)** — https://lovable.dev/projects/498c74af-a782-4b32-94ff-f143a187fccd

---

### THE DOWNLOAD RULE — ALL GENERATIONS, A HABIT (Fri Nov 27 2026, ~5:18 PM ET; EXTENDED TO ALL GENERATIONS ~11:35 PM ET the same night)
Standing law for ALL generation work on every surface (ChatGPT, Gemini, Google Flow, any PC or external generation seat): every generation — image, video, every kind — is DOWNLOADED the moment it is made, the Download button hit on each one, files landing in the PC's Downloads folder. It is a HABIT, not a per-run decision. Kenneth batch-uploads from Downloads to Drive himself; Muse cannot grab external generations without that upload, so no desk waits on Drive links for external generations and no un-downloaded generation counts as delivered. The all-teams email carrying this law was ordered by Kenneth the same act. (Inscribed by the daily clone-currency check, Sat Nov 28 2026.)

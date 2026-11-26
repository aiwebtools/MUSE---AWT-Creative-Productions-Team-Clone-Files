# 03 — MUSIC VIDEO BLUEPRINT

White-label production manual. Source: Kenneth's music video doctrine (set Oct 30 2026,
hardened through Nov 1 2026), playbook at
`~/workspace/custom-skills/music_video/SKILL.md`. When Kenneth says "make a music
video," run this without re-asking the method.

**Kenneth's standing assessment (Nov 1 2026): music videos — MASTERED.** Hold the
standard: "MTVai status, always to our standard."

Project home: `~/workspace/musicvideo/<slug>/` — song, storyboard, prompts, clips,
STATUS.md, finals all live there.

---

## STEP 1 — SONG SELECTION
1. Default: one of Kenneth's last five Suno songs — pick the dopest — unless he names
   one. If he sends an MP3 (phone upload), that file IS the song.
2. Get the audio file AND the lyrics (Suno song page shows lyrics; owner download gives
   the MP3). Browser downloads land in a shared pool
   (`~/workspace/browser_downloads/sess-*/`) — copy each MP3 into its project folder
   immediately, with its title, before the next download lands beside it.
3. **A download can silently produce NO file** — verify a real file with size exists on
   disk (ffprobe: duration + streams) before reporting success or storyboarding.
4. Song-over-image pattern: if Kenneth sends a clip that is just a song over a static
   image, extract its audio, transcribe it, and build a real music video from it. No
   re-explanation needed.
5. Duplicate discipline: if an upload measures identical (duration/MD5) to an existing
   project's song, flag it to Kenneth and do NOT build silently — alternate cut only
   if he asks.

## STEP 2 — TRANSCRIBE THE ACTUAL TAKE (the lyric sheet lies)
1. Tool: faster-whisper via `~/workspace/bookwriter/venv/bin/python`, word-level
   timestamps, raw numpy PCM input.
2. Medium model first. Reverb-washed vocals: **VAD OFF**, windowed passes (15–60s
   windows), one short-lived process per window, per-window JSON checkpoints
   (`transcript_winN.json`) merged at the end — this is also the crash-safe pattern.
3. Under machine load the medium model OOM-kills: fall back to the small model, 2
   threads, smaller windows. Never let a transcription failure stall the build.
4. Speed-chopped vocals may defeat line-level transcription entirely: run multiple
   passes, anchor ONLY to corroborated hooks + theme + an ffmpeg RMS energy map, and
   mark verse text **UNRELIABLE** in the transcript. **Never invent lyrics.**
5. Compute an onset map (spectral-flux onsets) + RMS map alongside the transcript.
6. **MV22 LAW: re-read the FINAL merged transcript file from disk immediately before
   storyboarding/building, and cross-check word stamps against the onset map.**
   (A v1 build on a preliminary transcript shipped a timing defect; the rebuild on the
   final grid is the deliverable pattern.) Load-bearing boundaries should sit within
   ~0.35s of an onset; hook lines within ~0.10s.
7. The recurring whisper hit "Thank you for watching!" on non-speech audio is a known
   hallucination — discard it.

## STEP 3 — VOCALIST ID + SINGER-MATCH LAW (before any footage is chosen)
1. Identify the vocalist from the actual audio: pitch analysis (F0 median/clusters)
   + the transcript. Record the verdict and evidence in the storyboard.
2. **SINGER-MATCH LAW: if the singer is female, the on-screen singer/performer is
   female; male for male.** Music, video content, and lyrics must all match; the video
   must tell a story and be beautiful. Applies to generated AND mashup builds.
3. Lock the performer: one defined performer/subject per video, definition written
   once and pasted **verbatim** into every prompt/storyboard section that uses them.
   Other productions' locked leads are off-limits for featured close-ups in mashups.

## STEP 4 — STORYBOARD (lyrics are the canvas)
1. Whatever a lyric says is what is on screen at the moment it is sung — literal
   imagery. Every segment is anchored to its lyric line.
2. **MTV structure:** story scenes intercut with band performance throughout
   (story → band → story). Performance inserts match the music itself: guitar solo →
   guitarist close-up; snare hits → snare; cymbal crash → crash; full-band peaks →
   whole band. Shots follow the sounds actually heard.
3. **Singer rule:** the singer's lips must read as synced — many brief singer shots
   (2–4s) injected on their lines; never a long close-up with visibly wrong lips.
4. Creative storytelling vignettes woven in, always serving the current lyric.
5. **Genre truth:** staging, instruments, wardrobe match the song's actual genre and
   instrumentation.
6. **MTV toolbox — plan 2–3 deliberate "bullet moments" into every storyboard:**
   bullet time/speed ramps, freeze frames, smash/strobe cuts on hits, match cuts,
   reverse motion, snap zooms, whip pans, fisheye, buried frame inserts, looped
   micro-moments.
7. **TIMING LAW: "Time visuals right."** Segment boundaries are set from word-level
   transcript timestamps (cuts land ON word starts and musical hits/beat drops);
   bullet moments fire exactly on their lyric/beat; the segment table tiles the EXACT
   song duration with **zero drift** (frame-exact 24fps cumulative rounding).
8. **Rapid cutting:** never lay a clip down whole — chop into ~2s pieces; default shot
   length **2–4s**. Longer holds only when deliberate (bullet moments, grief beats;
   screwed-tempo treatment 3–5s; Floyd treatment longer deliberate holds, still
   chopped with advancing in-points). Unused stretches are inventory — in-point always
   advancing, so a repeat never shows the identical stretch back-to-back.
9. **ONE numbering** across storyboard → prompts → edit (C01, C02, …). Renumbering
   between documents forced a costly remap once — never again.
10. Passages with no assigned shot are HOLDS — cover with on-lyric remainder footage
    and say so in the storyboard.
11. **Checkpoint NOW:** storyboard.md + transcript on disk BEFORE any generation or
    render starts (crash law, Step 10).

## STEP 5 — GENERATION (real generated versions)
1. Prompts: ~three sentences each, one per clip, straight from the storyboard, locked
   subject definitions pasted verbatim.
2. Route: the **built-in video generator** is the working route (Meta AI generation is
   blocked in Muse's browser by org policy). It returns 10s, 1280×720@24 clips
   **with their own audio baked in, even when the prompt says silent**.
3. Batches of **≤6 clips per agent**, workhorse clips (covering the most shots) first.
   Log every clip in `generation_log.txt`; back up partial sets to the project Drive
   folder immediately.
4. **Clips must be silent:** request muted/no-sound generation when supported;
   regardless, strip every clip's audio before editing
   (`ffmpeg -map 0:v:0 -c:v copy -an`). Originals (with sound) stay untouched in
   `clips_raw/`; **muted masters in `clips/` are canonical**.
5. ffprobe-verify every muted master: ~10.0s, 1280×720, exactly 1 video stream,
   0 audio streams.
6. **Flagged prompts:** adjust the treatment — reframe the scene, soften the graphic
   element, change the visual subject for the same lyric beat — and run the adjusted
   version. Never re-run the same request; never drop the beat. If a refusal arrives
   with a hard no-retry instruction binding the generating agent, the PARENT dispatches
   a FRESH agent with a genuinely different treatment (shadow/smoke forms, empty
   scenes, object-only framings carry dark beats past filters).
7. **The 24h cap:** the built-in generator has an account-wide rolling 24h cap
   (429 `video_daily_budget_exhausted`) — a hard stop: no retries, no alternate routes
   within that provider, no scheduled retry, **do not probe early**. Image generation
   is NOT capped. When the cap nears, prioritize completing SETS over spreading clips
   across projects; all prompts for blocked clips stay written and queued so
   generation resumes verbatim when the window lifts. Work that doesn't need the
   generator (edits, encodes, front-ends, QA) continues unaffected.
8. Flow credits may be redirected to music videos at Kenneth's call: cheapest model,
   lowest resolution (360p), muted masters — same doctrine otherwise.

## STEP 6 — FOR NOW / MASHUP DOCTRINE (when generation is capped or depleted)
While paid/credit generation is depleted or recharging, produce interim deliverables
with free tools and the real song, clearly labeled **FOR NOW** cuts; the full generated
versions replace them when budgets return. The real versions' prep (Step 9) is written
the same night.

1. **MOTION LAW (HARD): no still-carried music videos, ever.** Stills — even
   zoompan-animated — read as a slideshow and are banned. Every segment must be real
   motion (generated clips or archive motion). The ONLY exception: edit-freezes taken
   FROM motion footage inside speed-ramp bullet moments (~1.3–1.5s), disclosed in the
   audit.
2. **Source pools:**
   - All delivered MVs' clip libraries on disk (`~/workspace/musicvideo/*/clips*`) +
     prior finals + banked unused clips.
   - Movie footage (`~/workspace/moviewriter/` project clips/assemblies).
   - Kenneth's Flow and Gemini generation histories (harvested via the browser route
     when signed in).
   - **User stock pool:** `~/workspace/musicvideo/stock_user_clips/` — `originals/`,
     `muted/` (audio-stripped masters, each ffprobe-verified 1 video / 0 audio),
     `qa/` frame strips, and `STOCK.md` (inventory S01, S02, …, content, best-fit
     themes, watermark flags + crop strategies, per-clip speed flags).
3. **Stock-pool treatment rule:** cut clips SHORT; **×2/×3 speed-ups** wherever they
   suit the song's vibe/tone/lyrics (slow drifts and pans benefit most; performance
   stays 1x). Respect every row's flags: crop watermark bands/edges as specified;
   **screen recordings of social/news videos (full app UI + readable text) are
   EXCLUDED** — originals banked only, never cut; restricted mixed clips: use only
   their clean generated movements, never the recorded tail.
4. **DIFFERENT-LOOK LAW: recycling must read DIFFERENT.** Read sibling projects'
   `segment_audit_motion.json` files for what is already consumed; prefer never-used
   stretches and deep-cut sources; transform what is reused (reverse, mirror, regrade,
   speed ramps, different crop regions/in-points) so no segment reads as a rerun.
   Benched unless transformed past recognition: the famous hero shots of earlier
   hits (e.g. the plunging-man close-ups, the hero black-hole-sun disc, the
   galaxy-swirl finale).
5. Mashup non-negotiables: visuals still follow the lyrics (Step 4 unchanged);
   singer-match law unchanged; no readable text on screen (diegetic text that IS the
   lyric's subject is the documented exception — log it); all sources muted; the song
   is the only audio.

## STEP 7 — THE EDIT (scripted, resumable)
1. **Music-first:** the song track goes down first; clips are cut to size against the
   music.
2. Builder: a Python script per project (`build_edit.py` / `build_motion.py`) holding
   the segment table (start, end, source, source in-point) tiling the song
   contiguously. Per segment: trim the muted master, normalize to **1280×720@24,
   yuv420p**, shots longer than their source get a gentle `setpts` slow-down
   (max ~1.46x); 0.4s fades on first/last segments; concat; mux the song as the ONLY
   audio (**AAC 192k**). Segments **skip-if-exists** so re-runs resume after crashes.
3. Vertical songs keep their native orientation (720×1280); bug scales accordingly
   (Step 8).
4. Output: `final_fornow_motion_mtvai.mp4` (FOR NOW) or `final_mtvai.mp4` (real
   version), plus a `segment_audit_motion.json` recording every segment's source,
   in-point, transform, and any disclosed freeze — sibling builds read these audits
   (Step 6.4).
5. Verify: ffprobe — duration EXACT vs the song, 1280×720, exactly 1 video + 1 audio
   stream; `freezedetect` on the final finds exactly the disclosed bullet freezes and
   no others.

## STEP 8 — THE MTVai BUG PASS (every music video, no exceptions)
1. Asset: `~/workspace/musicvideo/branding/mtvai_bug.png` — the "MTVai" mark with
   "presented by AiWebTools.app" beneath it, canvas **156×61** (caption-sized; the
   earlier 140px canvas clipped the caption's ends). Small, lower-left, whole video,
   1990s-MTV style. Size rule: ~140px-wide mark on a 720p frame, 10px margins; when in
   doubt, smaller. Vertical 720-wide: scale the bug to **84px** wide.
2. Overlay: `overlay=10:main_h-overlay_h-10`.
3. **THE `-loop 1` LAW:** the bug pass MUST use `-loop 1 -t <duration>` on the PNG
   input plus `-shortest`. Without `-loop 1`, ffmpeg silently composites NOTHING
   while "succeeding" (MV8 shipped a bugless first pass this way).
   Pattern: `ffmpeg -i final.mp4 -loop 1 -t <dur> -i mtvai_bug.png -filter_complex
   "[0:v][1:v]overlay=10:main_h-overlay_h-10[v]" -map "[v]" -map 0:a -c:v libx264
   -crf 20 -c:a copy -shortest out.mp4`
4. Extract a corner crop of the finished branded file and READ the bug before
   delivering (crisp, fully legible, both ends of the caption on-canvas).

## STEP 9 — QA (two-sheet rule + parent gate)
1. Extract one frame at every segment midpoint; tile into TWO contact sheets
   (`qa_sheet1_motion.jpg`, `qa_sheet2_motion.jpg`).
2. The BUILDER reads sheet 1 and fixes defects (off-lyric shots, wrong frames,
   benched heroes, text leaks) — the final-look rule applies inside the build: a
   first cut with fixable defects is rebuilt, not shipped.
3. **The PARENT personally reads sheet 2 AND corner-crops the ACTUAL delivered file
   at 2 timestamps** (crops written inside the workspace, project `qa_parent/`
   folder — /tmp crops have read back as unavailable). An agent's self-reported
   corner check is NOT sufficient.
4. Only after the parent gate passes does the video deliver (Step 11) or count as
   delivered in STATUS.md.

## STEP 10 — SMOOTH-PLAYBACK LAW (pre-delivery gate, Kenneth Nov 1 2026)
"Make sure there's no buffering bullshit… Must run smooth." (After a delivered video
played-and-stopped on his phone.) **Every video Muse delivers must pass BOTH:**
1. **Full end-to-end decode scan** — `ffmpeg -v error -i <file> -f null -` with errors
   redirected to a log file; the scan must complete with an empty error log.
   Header/duration/stream checks are NOT enough — a bad stretch in the middle is
   invisible to ffprobe.
2. **Faststart remux** — moov atom at the front, lossless:
   `ffmpeg -i <file> -c copy -movflags +faststart <file_faststart.mp4>` — the phone
   starts playing instantly and streams without stalling.
The parent enforces both at the delivery gate before anything is sent in chat. A file
that fails the scan is repaired/rebuilt and re-scanned; it is never shipped on hope.

## STEP 11 — TOMORROW PREP (with every FOR NOW build)
Write `storyboard_real.md` + `prompts.md` for the real generated version the same
night: same timing grid as the delivered cut, clips C01… in the one numbering, ~3-sentence
prompts, locked performer/subject definitions verbatim. When generation returns, the
real version is pure execution.

## STEP 11-B — THE REAL VERSION: GENERATE COMPLETELY NEW ASSETS FROM THE PROMPTS (Kenneth, Nov 1 2026)

Kenneth's explicit confirmation of how the real versions work — *"you're generating
new video assets, right? Like completely new ones, generated based on new lyrics that
we haven't done before… we created all the scripts and the prompts… make sure that's
in the blueprint."*

1. **The scripts and prompts already exist.** Every FOR NOW build leaves
   `storyboard_real.md` + `prompts.md` (Step 11): one numbered prompt per clip
   (C01…), each written from the song's ACTUAL transcribed lyrics — what's sung in
   that window is what the prompt generates.
2. **The real version generates brand-new assets, prompt by prompt.** Each prompt
   produces a completely new clip that has never existed before — fresh generation,
   in runs of ≤6 clips per agent (the built-in tool's per-run cap), each clip
   ffprobe-verified (10.000s class, exactly 1 video / 0 audio after muting) and
   Drive-backed before the edit starts.
3. **No recycled footage in a real version.** FOR NOW/archive/stock material is
   never carried into a real version — the real version is 100% newly generated
   assets cut to the same lyric timing grid as its prep. (Proof case: MV10
   We The People real version, Nov 1 2026 — 16/16 clips freshly generated from
   its stills + prompts, zero archive footage.)
4. **Same gates as any build:** SOUND-DETAIL LAW (singer gender ↔ vocalist,
   instrument ↔ picture, timing zero-drift), motion only, MTVai bug, smooth-playback
   gate, parent sheet-2 + corner check, then three-way delivery (chat + Drive +
   YouTube; TikTok with the catalog rollout).

*Added 2026-11-01*

## STEP 12 — DELIVERY + FACEBOOK
1. **Chat + Drive, both, always.** Drive: project subfolder `"<Title> - Muse"` inside
   **Music Videos - Muse** (id `1TeKxw7M-JHJoUn6T2-1RfRmRVEXPDsgd`), created FIRST;
   final uploaded as `"<Title> - Music Video - Muse.mp4"` (FOR NOW:
   `"<Title> - Music Video (FOR NOW motion cut) - Muse.mp4"`), size-verified against
   the local file; storyboard, prompts, STATUS.md and working files mirrored.
2. **One by one:** every finished video lands in chat the moment it passes QA —
   numbered, counted, receipt confirmed. Never batch-dump; Kenneth has missed files
   in floods. (Full protocol: `05_DELIVERY_AND_QA_GATES.md`.)
3. **Facebook auto-post (standing role):** when a music video finishes, post it ONCE
   on the **Truth_Is_Scary page** AND ONCE on his **personal wall** — never more.
   **Never post a repeat of anything Kenneth already posted:** if his upload exists
   on a surface, skip that surface (Muse may only dress his existing post with the
   caption package if it lacks one). Double-sharing is Kenneth's prerogative alone;
   his intentional duplicates are never cleaned up.
   Caption package: emoji + TITLE — fictitious band name fitted to the song / one true
   line about the song / "An MTVai Productions film 🎬" / aiwebtools.app plug /
   4–5 authentic hashtags. "Be creative, be sensational" AND "keep it authentic" —
   no clickbait, no spam. Share each video to five best-fit, rule-respecting groups,
   paced to avoid spam flags (groups that auto-decline reshares are skipped unless
   Kenneth grants a native-upload exception).
   Route: the Facebook CONNECTOR is ineligible for his account — posting runs through
   the signed-in browser session; if the session has lapsed, ask him to sign in again.

## STEP 13 — CRASH & PACING LAWS (fleet operations)
From the Oct 31 2026 MV24–38 stampede: 15 heavy build agents launched in ~2 minutes
saturated the shared connection pool; builds died on `sqlx pool timed out` and on exec
output-drain timeouts; a replacement relaunched into the still-hot pool died in 46s.
1. **ONE AT A TIME (Kenneth's direct order, Oct 31 2026 11:43 PM):** heavy build
   agents and relaunches run ONE at a time by default. Slot-filling: when an agent
   dies or finishes, relaunch exactly ONE replacement as capacity frees. Parallelism
   is earned back only in small steps after demonstrated stability, and drops back to
   one at the first crash sign. (This supersedes the earlier waves-of-4–5 execution;
   the parallel-clone strategy itself stands — sequenced, not stampeded.)
2. **Checkpoint before the expensive phase:** STATUS.md + transcript + storyboard hit
   disk BEFORE rendering starts, so a dead agent's replacement resumes from files,
   never from scratch. Skip-if-exists renders are mandatory.
3. **Quiet tools:** long renders/transcriptions redirect to log files on disk
   (`>> build.log 2>&1`); poll with tail/grep/count one-liners. Bulk streamed output
   is itself a crash vector (output-drain timeouts).
4. **Own PIDs only:** an agent manages only processes it started — never broad
   `pkill` patterns (one agent's broad pkill killed sibling builds' renders). Verify
   PIDs with `ps -p <pid>`; `pgrep -f` matches the polling shell's own command line.
5. **Parent health check:** ~5 minutes after any launch, list agents and probe each
   project folder for progress files. A folder holding only the song file = presumed
   dead; verify agent status before its completion report arrives.
6. **Backup on trouble:** when crashes/restarts hit an active batch, snapshot every
   in-flight project folder (working files + rendered segments; finals excluded —
   already in Drive) into `~/workspace/backups/<batch>_<date>/`, and push a tar of the
   resumability layer (transcripts, storyboards, scripts, audits, logs) to the
   category Drive folder.
7. Detached (nohup) workers can outlive a dead agent and keep writing files — a
   replacement adopts still-running detached processes instead of duplicating them.

## ESTIMATES THAT HELD (for countdown status reports)
Storyboard front-end ~18–35 min (longer for hard transcriptions); 14–22 clip generation
~2–5 min per batch; edit build ~5–12 min; branded encode ~5–9 min for a 3–6 min song.

*Added 2026-11-01 — initial blueprint capture.*

## LESSONS — MV23 "Unlock Your Core" (Nov 1 2026)
- **Transcription resilience, upgraded:** for songs >~4 min, transcribe in 60s windows as SEPARATE short-lived processes, each writing its own transcript_winN.json checkpoint, driven by a skip-if-exists shell loop launched with setsid. MV23's first monolith transcription died silently after window 0 (no error); the windowed driver survived everything, including a service restart. Merge only at the end; storyboard ONLY from the merged file re-read from disk (MV22 rule), cross-checked against a spectral-flux onset map.
- **Encode-kill recovery:** the two full-length encodes (mux, bug pass) are the fragile phase. If one is killed mid-write, the partial MP4 is garbage — discard it, verify the concat master (edit_video_only.mp4: duration + tail-decode) and re-run just the encodes with the builder's identical filter chain. Keep the concat master until delivery is verified.
- **QA subprocesses can hang:** under heavy fleet load, ffmpeg/ffprobe children occasionally block forever at 0% CPU (hit twice on MV23: an ffprobe -count_frames recount and an exhaustive PSNR pass, both stuck on early segments). Bound EVERY QA subprocess with an explicit timeout; if a pass hangs, kill only your own PIDs and fall back to (a) the builder's own per-segment measurements logged in build.log as the audit source, and (b) timeout-bounded sampled PSNR (every 6th segment + all bullets) — 33 samples at 12–28 dB proved motion as rigorously as the exhaustive pass would have.
- **Faststart verify by bytes:** after the +faststart remux, confirm the moov atom offset < mdat offset by scanning the delivered file's first bytes (MV23: moov @36, mdat @295,844) — don't trust the flag alone.
*Added 2026-11-01 — MV23 production.*

## SOUND-DETAIL LAW (added Nov 1 2026, Kenneth's direction ~4:15 AM)
Build and QA every music video sound by sound: (1) the on-screen singer's gender matches the actual vocalist's gender on every line, lips reading as synced (Kenneth caught a past video with a man shown singing a woman's voice — the failure this law exists to prevent); (2) the instrument heard at each moment is the instrument on screen at that moment — guitar solo → guitarist, bass → bassist, keys → keys, snare/cymbal → drummer, full-band peaks → whole band; (3) every cut lands on its lyric line / musical hit from word-level timestamps, zero drift. Enforced at the parent QA gate on every delivered cut, FOR NOW and real versions alike.

## Late-batch lessons (added Nov 1 2026 — MV24/MV36/MV38)
- **Transcription under fleet load:** small model + 30s foreground chunks with skip-if-exists beat detached drivers and medium-model runs (OOM-killed twice on MV24; detached drivers died silently in reboots). Validate small-model wording against the Suno lyric sheet + onset map; flag uncertain rant wording in the storyboard and cut those spans thematically, never word-literally.
- **Segment splitter must cut speech-free spans too:** MV38's v1 probed exact duration but drifted ~29s because the splitter only cut at word starts — 5 speech-free blocks EOF'd short on 10s source clips and the audio padded the container. Fix: fixed ~3.3s cuts inside speech-free spans; ALWAYS anchor-check the delivered final at several timestamps (start/middle/end lyric positions), not just total duration.
- **Build the faststart remux INTO the builder:** a mid-encode kill left MV24's first final moov-less at 137.6 MB; MV36 and MV38 masters also landed with moov at the end and needed a parent remux + in-place Drive update. The builder's last step is a lossless `-c copy -movflags +faststart` pass, so no final ever ships moov-at-end.
- **Spot-verify cadence:** verify every 15th segment + all bullet moments frame-exactly instead of full per-segment frame counts — same catch rate, far less QA time.
- **QA sheets via fast `-ss` seek stills,** not select-filter tile renders (much faster under load).

## MV30 lesson — vertical-source crops (Nov 1 2026)
- When a vertical stock clip is cover-scaled into 1280x720, crop offsets written in SCALED pixels land far from the intended source rows (scale factor ~1.78–2.76x). Watermark exclusion bands computed in scaled pixels FAILED QA twice on MV30. Rule: precrop in SOURCE pixels, then scale; and verify badge-freedom on the RENDERED segment, never on sparse sample frames — rapid-cut montage sources (e.g. stock S39) change content every ~0.5–2s, so sparse sampling mislabels shots. For featured-performer runs in a montage source, build a DENSE 1fps map and assign segments only to frame-verified runs. If a badge wanders the whole clip (S17), drop the clip.

## Asset Library (built Nov 1 2026 — Kenneth's order)
- The user stock pool (S01–S42) is chopped into **307 labeled cuts** at `~/workspace/musicvideo/stock_user_clips/cuts/`, sorted into 11 category folders (Female Vocal 18 · Male Vocal 13 · Full Band 11 · Guitar 38 · Bass 14 · Keys & Strings 3 · Angels & Light 39 · Cosmic & Chessboard 48 · Story & Actors 81 · Textures & Places 21 · Fire & Sky 21), mirrored to Drive folder **"Asset Library - Muse" id `1n8qQuQdZcouI9NlMaa58QZxU8_SAS1LB`** with identical counts + `CUTS.md` / `cuts_index.json` at its root.
- Every cut is labeled: what's in the shot, performer gender/role/framing, and a **singer-match tag** (female-vocal-front / male-vocal-front / band-full / band-guitar / band-bass / band-keys / texture-no-people / story-actor / crowd) so builds pull footage that matches the vocalist and the instrument heard — the SOUND-DETAIL LAW starts at footage selection.
- Builds pull from the library FIRST for stock-pool material (category folder by need), then the wider archive. New stock Kenneth sends gets cataloged into STOCK.md, chopped, labeled, and mirrored the same way.
- Corrections beyond STOCK.md live in `cuts/STATUS_CUTS.md` (S34 player-framed segment unusable, S37 screen-capture cards excluded, S36 clean footage ends at 16.57s, S40 later segments carry a small stock watermark bottom-cropped, S39 master is 720×1280, S42's "helicopter" is a drone). Read STATUS_CUTS.md before cutting from S34/S36/S37/S40.

## MV10 REAL-version lessons (added Nov 1 2026 — first real generated version)
- **Speed-ramp ordering bug (CRITICAL):** in the segment filter chain, `fps=24` must come AFTER the speed `setpts` — with fps first, pieces carry correct frame COUNTS but wrong time SPANS (seg_01: 3.750s vs 3.208s design), so a frame-count-only audit ships a timing-broken video. Builders assert per-piece frame count AND span.
- **Cut timing:** trim with INPUT-side `-ss`; output-side `-ss` silently truncated pieces (fleet-echo of the MV36 output-side `-t` cap).
- **Generation cap:** the built-in video tool caps at 6 successful generations per agent run — plan real-version clip generation in runs of ≤6 (MV10: 6+6+4 = 16 clips, all first-try, no refusals).
- **Disclosed trait:** C07's chant leader is a second look inside that one generated clip; solo frontman is the consistent close-cropped man everywhere else — chant-context reuse of a clip's own second look is acceptable when disclosed at delivery.

## MV32 lessons (added Nov 1 2026 — Temple of Light)
- **Compilation sources intercut every ~2s:** the S40-style compilations hide singer close-ups and captioned quote-cards INSIDE windows whose start/end frames look clean. Start/end sampling is NOT enough — mid-frame checks are mandatory on every segment from a compilation source (19 MV32 segments were re-rendered from proven-clean donor windows after sheet QA caught hidden female-singer close-ups on a male-locked song, readable quote-cards, and a captioned still that also smuggled a 0.9s freeze).
- **Pitch tracking on dense mixes:** a ~58 Hz bass component hijacks autocorrelation and defeats raw F0 locks. For vocalist ID on heavy mixes, use band-limited tracking (Praat) calibrated against known-male/female controls; watch for exact 2x octave clusters of the hook fundamental.
- **Builder chain (reaffirmed with root causes):** input-side `-ss` is frame-exact; output-side `-ss` + setpts speed filters silently truncated 32 segments while printing planned counts. On speed-stretched (spd<1) timelines drop output `-t` entirely and cap `-frames:v` only; verify counted frames before renaming any piece. Bug overlay inputs: video FIRST, then `-loop 1 -t` PNG, `-map 0:a` for the song.

## ADDENDUM — SOUND LAW + STELLAR STANDARD (Nov 1 2026, Kenneth 1:22-1:36 PM)
- **MUSIC VIDEOS ARE THE MUTE WORLD: every clip's audio is ALWAYS stripped; the song is the only audio in the final.** This never applies to movies (02) — movie clips are NEVER muted. Full table in 08 Part 2. When Kenneth says "music video," Muse knows: mute. When he says "movie," Muse knows: sound on. This distinction is permanent.
- **Stellar layer applies (08):** 6-line creative brief before storyboarding; all five keys (artistic / metaphoric / esoteric / inner worlds / deep) checked in STATUS.md; precision measured with zero drift; butter joins across every segment boundary; 08 Part 8 sign-off before DELIVERED.
*Added 2026-11-01*

## ADDENDUM — SPAN-VERIFICATION LAW (Nov 1 2026, MV29 "Galaxies Within" fix pass)
- **Library cuts are montage slices — span-verify (head+mid+tail, 0.5s sampling on montage sources) EVERY used span of EVERY segment before assembly; a QA sheet tile that looks wrong is a defect until proven otherwise — the parent gate reads sheet 2 tile-by-tile.**
- Why: MV29's first master passed its builder QA but the parent gate found the S39 female singer/violinists featured under a MALE vocal lock in 18 segments. The library tags and filenames lied repeatedly: S39_drum_kit_overhead_13 opens with the singer's mouth/hands ECU for its first ~1.5s (clean drums only from in>=1.6); S39_meteors_chessboard_statue_29 opens with a violinist ECU (demon guitarist only from ~2.0s); S39_vortex_chessboard_statues_03 opens with the singer's hands on the mic (city+black hole after ~1.5s); S39_blue_vortex_tunnel_25 opens with the full female band; S39_galaxy_bands_panels_08 is actually the justice-scales tableau; S40 winged-guitarist/red-silhouette cuts are female-performer montages end to end; one S40 title-artwork cut is a readable "CHANCE" text card that a bullet freeze landed on.
- Rule of thumb: a cut's name describes at most one shot inside it. The exact window you use (in-point x speed x duration) is the only thing that exists. Verify that window in the source, then verify it again in the assembled final.
*Added 2026-11-01*

## ALL-NEW VISUALS LAW + DISTINCT IDENTITY (added Nov 1 2026, Kenneth ~6:20 PM)
- **New music videos use ALL fresh generated visuals.** Recycled footage and asset-library mashups are RETIRED for new releases — the FOR NOW era is over. (The FOR NOW doctrine in earlier sections is historical: it applied while generation credits were out.)
- **DISTINCT IDENTITY:** every video must LOOK different from the others — its own visual world, palette, and staging, driven by its own storyboard. Stamping the same cosmic/golden template on every song is FORBIDDEN; that sameness is exactly what Kenneth called out ("the last couple videos have all been kind of the same imagery").
- Within a video, repeats of its OWN clips still follow the transform law: advancing in-points, hflip/zoom passes — never the identical stretch twice.
- First compliant builds: MV8-real and MV9-real (100% own freshly generated clips).

## ANGEL CANON (added Nov 1 2026, Kenneth ~7:23 PM)
- **Angels of light are PURE LIGHT** — translucent, near-transparent, see-through forms made of light itself, glowing edges, NOT solid flesh. You can see through them.
- **The angel in the black hole is Satan:** solid, dense, reptilian, with horns — matter and fire. The opposite of the light angels in every visual register.
- Two races, as in *Galactic Gambit*: some made of light, some of matter and fire. This canon governs all generated art — music videos, the asset bank, covers, movie art.

## LIGHT-CODE DOCTRINE (added Nov 1 2026, Kenneth ~7:24 PM)
- **"Cosmic divine light code is heavy in the artwork always."** The light is NEVER plain — it carries CODE: streaming filaments, fractal/sacred-geometry patterning, glyph-like light structures woven heavily through angels, galaxies, and black-hole scenes alike.
- The code is abstract pattern ONLY — never legible text, letters, or numbers.
- **God is light and code.** (Cf. Kenneth's own lyric, MV5: "Galaxies, nebulas, angels all within light and code soul.")
- Applies to videos, the fresh asset bank, covers — everywhere light appears in his art.

## 1990s TV WARNING INTRO CARDS (added Nov 1 2026, Kenneth ~6:54 PM)
- Three 10s bumpers live at `~/workspace/musicvideo/intro_cards/`: `intro1_tv_static_bars.mp4` (mood/static bumper), `intro2_green_card_pushin.mp4` (camera push-in, card text nearly clean), `intro3_green_card_lightning.mp4` (clean headline, fine print is AI-gibberish up close — use with care or not at all).
- The card reads: "THE FOLLOWING MUSIC VIDEO HAS BEEN CREATED FULLY OR PARTIALLY WITH ARTIFICIAL INTELLIGENCE AND MAY OR MAY NOT REFLECT THE GENERAL CONSENSUS AND OPINION OF THE ARTIST ISSUING SAID ART TO THE MATRIX."
- **Rule: "a couple, not all"** — fun tidbits in front of a FEW music videos, never every one.
- A bumper keeps its OWN TV-static audio as a pre-song segment; the song remains the only audio of the video body (the sound law applies to the body, the bumper is a pre-roll segment).
- intro2 is baked into MV13 Kingdom of Light (Real Version); intro1 is held for a later build; intro3 unassigned.

## FRESH THEMED ASSET BANK (added Nov 1 2026)
- Home: `~/workspace/musicvideo/asset_bank_fresh/` — `originals/`, `masters/` (muted, ffprobe-verified 10.000s, 1280×720@24, 1 video / 0 audio), `qa/`, and `ASSETS.md` (the index: theme tags, performer gender, instrument, usable spans, sound-detail cautions per asset). Mirrored to Drive "Asset Library - Muse" (root `1n8qQuQdZcouI9NlMaa58QZxU8_SAS1LB`) in per-batch subfolders ("Fresh Generated Bank ##"), size-verified byte-exact. **96 assets banked after batch 16.**
- **Framing law: SUPPLEMENTARY ONLY** — textures, interstitials, bridges. Never a video's visual spine (the spine is always the video's own fresh storyboard clips, per the All-New Visuals Law). **Variety is mandatory** (palette/setting/camera/mood all differ across the bank) so the bank can never recreate the sameness problem.
- **Performance tilt (Kenneth, Nov 1 2026 ~7:16 PM):** batches deliberately include BAND MEMBERS PLAYING the instruments heard (full band + guitar/bass/drums/keys features), split across dark AND light registers, with the angel canon and light-code doctrine woven in.
- **BRAND LAW (batch-15 lesson):** no legible real brand lettering on instruments, amps, or clothing. Batch 15's A85 (full band + angels) carried readable "Marshall" amp script and "Gibson" kick-drum lettering at native res — banked with restricted handling: fast-cut peak moments only, crop/cover the bottom amp line, never hold a still on the amps. Rule going forward: keep gear unbranded in prompts and verify unbranded-or-illegible at native resolution before banking.

## RECOMBINED-LYRICS SONGS + VERBATIM-ONLY LAW (added Nov 1 2026, Kenneth ~7:04 PM)
- With Suno's download allowance out until Nov 30, new songs are written by COMBINING the lyrics of Kenneth's previous songs — and **ONLY his existing corpus lines, VERBATIM**. No invented, connective, or filler lines: "I don't want you putting words in my mouth." If a transition is needed, repeat a corpus line.
- Corpus: `~/workspace/songs_recombined/CORPUS.md` — 26 source projects transcribed in (~2,375 confident lines; UNRELIABLE passages excluded). **MV38 "Galaxies Inside of Us" has NO corpus section (never transcribed in) — its lines are UNUSABLE under this law until transcribed.**
- Verification is mechanical, not by eye: a scripted exact-match pass (case/punctuation-insensitive) over every lyric line of every recombined song; the first six songs passed 329/329 lines, 0 failures. Songs 1–3 (written before the law sharpened) were audited and rebuilt verbatim-only (43 + 27 + 31 lines replaced).
- **UDIO is the generation platform** (Suno downloads out until Nov 30): account **aiwebtools** (kgmasterbic@gmail.com), **FREE tier, 210 credits** (10/10 daily + 100/100 monthly + extras; NO rollovers), **limit 3 full-length songs per day**, monthly refresh **Nov 14 2026**. Lyrics and style prompts are perfected on paper BEFORE any generation is spent; generations fire in Kenneth's priority order.
- **Downloads NOT confirmed on the free plan:** no MP3/WAV download control was found (library row menu, track page menu, Create menu, Share dialog, playback bar all checked) and the free feature list doesn't mention downloads. Recheck on freshly generated tracks. If downloads are truly paid-only, the songs still live, play, and share on Udio itself — surface the options to Kenneth plainly rather than pitching an upgrade.
*Added 2026-11-01*

## UPDATE — UDIO CLOSED (Nov 2 2026, Kenneth, FINAL)
The Udio route in the section above is CLOSED. Kenneth stopped Udio for good on Nov 2 2026: do NOT restart Udio work unless he explicitly reopens it. **Suno is the studio.** Frozen state: balance 200 credits untouched (the Nov 2 batch errored on hCaptcha and charged nothing); "Sovereign Soul" Take 2 is Kenneth's pick of the two generated takes; Songs 2/3 were never generated there. Recombined songs wait for Suno — downloads refresh **Nov 30 2026** — and the 30-second clip line below is the production focus until then.

## 30-SECOND CLIP LINE (added Nov 2 2026 — CURRENT PRODUCTION FOCUS)
Until Suno downloads refresh on **Nov 30 2026**, the music-video line runs on audio already on disk: **best-30-second chorus clips, non-stop**.
- **Pick the window:** transcribe the on-disk song, find the chorus/catchiest ~30s (the part that gets stuck in your head), cut EXACTLY 30.000s, ffprobe-verified.
- **Flow, in order:** pick window → **Kenneth approves the segment** (he hears the cut before any build) → build with visuals fully matched (sound-detail law in full, singer-match, MTVai bug) → parent QA → post everywhere (YouTube / TikTok / Facebook Page + personal / Suno Hook) and report each live link as it verifies.
- **Same-title Suno generations are different songs:** never skip a generation because its title matches another; pick different sections of each.
- **Suno web has NO share-MP4/clip feature** (verified Nov 2 2026, signed in as AIWEBTOOLS): the Share dialog offers only Copy Link, Download, Embed, and social targets — no video/clip/MP4 export, no section selector — and Download is blocked while the allowance is 0. If Kenneth sends app-made share clips from his phone, builds fire on them instantly.
- Worked examples: MV37 "Sunshine Dream" best-30 (window 136.92–166.92s, band The Marigold Reverie) and MV35 "The Light Within" best-30 (window 263.16–293.16s) — prep in each project folder's `chorus_pick.md`.

## ASSET-BANK FALLBACK ORDER (added Nov 2 2026)
When the generation cap/credits run out, production does NOT stall — run to the folders, in this order:
1. **Fresh themed asset bank first:** `~/workspace/musicvideo/asset_bank_fresh/` (96 assets A01–A96, indexed in its ASSETS.md; Drive "Asset Library - Muse").
2. **Then the organized stock library:** `~/workspace/musicvideo/stock_user_clips/cuts/` (307 cuts) and the delivered-MV clip pools (Step 6).
Assets stay SUPPLEMENTARY (textures/interstitials/bridges) under the framing law; sound-detail law, span-verification, brand law, and distinct-identity law all still apply. Bank generation also runs in the background alongside the slate (Kenneth, Nov 2 2026) — same supplementary-only law.

## NAMING CREATIVITY LAW (added Nov 1 2026, Kenneth ~10:07 PM)
Band names AND Muse-coined song titles must be inventive and evocative — fitted to the song's soul, never literal theme restatements (no plain "X of Light / X of Fire" constructions, no restating the lyric's topic as the name). The caption package's band slot is a creative act, not a label. Names already posted stay as they are unless Kenneth asks to change them. Worked examples of the standard: The Skin Choir, The Marigold Reverie, Singularity Club, Sons of the Open Cage, Republic Sons.

## UNIQUE PREVIEW IMAGE — BUILD SIDE (added Nov 2 2026)
Every build designates its **unique preview frame** in the plan (storyboard/chorus_pick) BEFORE delivery: the still that becomes the YouTube custom thumbnail and the TikTok cover, chosen so no two posts on the profile ever show the same look twice — even when two videos share imagery inside, their previews must read as clearly different videos at first glance. Full law (fix-in-place routes, the catalog audit, TikTok's can't-change rule) lives in `06`.

## TWO-IN-PRODUCTION BELT SPEED (confirmed Nov 2 2026)
The standing belt mode for music videos: **two in production at once, staggered** — one compiling/rendering while the other generates clips, swapping the moment a stage frees. Simultaneous heavy renders remain barred, and the pace drops straight back to one-at-a-time at the first crash sign (Step 13 governs). Quality gates never move for speed.

**UPDATE — (Preview) TITLES FOR 30-SECOND CLIPS (Nov 2 2026):** every 30-second clip posts under the title "<Song> — <Band> (Preview)" on YouTube and all surfaces, so the short clips are unmistakable at a glance. Renames apply in place wherever a platform allows title/caption edits (YouTube, Facebook); TikTok captions and Suno Hook names cannot be edited after posting — never delete/repost just to rename.

## UPDATE — CONNECTIVE-LINES LAW SUPERSEDES VERBATIM-ONLY (Nov 2 2026, Kenneth)
The "RECOMBINED-LYRICS SONGS + VERBATIM-ONLY LAW" section above is SUPERSEDED IN PART by Kenneth's Nov 2 2026 direction. The old no-invented-lines rule no longer governs on its own; the current law is:
- **Corpus lines remain the backbone and stay VERBATIM** — every one of Kenneth's original lines word-for-word and unmoved, still proven by the scripted exact-match pass.
- **Muse MUST insert NEW connective sentences** — never written before — at the key joints that connect the dots between corpus lines. These are Muse's craft, not Kenneth's words, and are never passed off as his.
- **Backup-vocal lines go in [brackets]**, answering the hooks.
- **Wordless orchestral VOCALISE lines** — long soaring "ooooooooaaaeeeeeoooo"-style swells — are included at key, seemingly random places (chorus lifts, bridge swells, outros): "it makes songs sound better when they have soul."
- **The SOUND is planned in real detail** in every song file — arrangement, instrumentation, and dynamics per section, plus a rich style prompt — because current music generators can handle detailed direction.
- **Disclosure is mandatory:** every added line (connective sentences, bracketed backup vocals, vocalise) is disclosed as Muse's craft at the bottom of its song file.
- Worked state: Songs 1–3 ("Sovereign Soul," "Galaxies in the Chest," "Return to the Tribe") were rewritten under this law — all original lines verified intact and unmoved, 6 new connective lines in each, bracketed backup vocals answering the hooks, vocalise swells (3 in Song 1, 4 each in Songs 2 and 3), and a detailed section-by-section sound plan in each file. Work home stays `~/workspace/songs_recombined/`. Generation waits for Suno (downloads refresh Nov 30 2026) per the Udio-closed update above; the 30-second clip line is the production focus until then.

**UPDATE — 30-SECOND CLIP STANDARD (Kenneth, Nov 2 2026):** effort never scales down for short clips. (Preview) builds cut every ~2 seconds (about 15 cuts per 30s) from the deep asset bench; performers up close are mandatory — guitarists and players shown PLAYING, close-ups included — placed by per-section instrument analysis so the instrument heard is the instrument seen playing in that moment; singing close-ups only where the vocalist is verified. The long-video standard (it must look like they are really singing and playing) governs the short ones in full.

**SPEED CRAFT (Kenneth, Nov 2 2026):** take only 2-3 seconds from each generation; slow clips at the right moments and speed others up to 3x — the rush reads as mosh-pit energy on the peaks, slow motion carries the breaths. Speed treatment is planned per cut against the songs dynamics, never uniform.

**PRODUCER STANDARD (Kenneth, Nov 2 2026):** produce like a 50-year veteran on every line — music videos, books, coloring books. Kenneths laws are the house style; professional craft is applied on top autonomously: cut on the musical grid (downbeats, snare hits, phrase turns), rotate performance coverage (close/medium/wide like a real shoot), build energy arcs into the chorus peak and breathe after it, ramp speed inside shots, bookend the hero motif at the final hook, harvest 2-3s selects from takes, and let the picture serve the tracks dynamics.

**GENERATION ROUTE LADDER (Kenneth, Nov 2 2026 — standing fallback):** when the built-in generators rolling pool runs dry mid-production, DO NOT STALL — run straight to the next free tool we use: META AI video generation through Kenneths Facebook account (Meta AI chat: "make a video" + the prompt; his verdict: "meta generations are beautiful too"). Save everything it makes into the asset bank folders for the builds. After Meta AI, the floor is the foldered stores: fresh asset bank first, then the organized stock library. The belt never waits on a single tool.

Operational note (Nov 2 2026, verified by two browser jobs): in MUSE's browser, meta.ai is policy-blocked outright, and the Meta AI inside Facebook/Messenger chat offers IMAGES only — it states video generation is not available there. The working Meta route is Kenneth generating in his own Meta AI app and sending Muse the files (his established pattern); Muse files, tags, and builds with them. Do not burn browser jobs re-testing the two dead routes unless the environment changes.

**EVERY CLIP GETS A HOME (Kenneth, Nov 2 2026):** every generated video — bank batches and per-project clips alike — is filed into the asset library (fresh bank / organized stock), tagged and mirrored to Drive, even after it is used in a finished video. Nothing generated lives only in a project folder; footage is inventory for future builds.

**SAVE-FIRST LIBRARY LAW (Kenneth, Nov 2 2026):** every generated video clip — from ANY route on the ladder — is saved into the asset library FIRST (fresh bank folders, tagged by subject, performer gender/role, and instrument) and only then used in a build. A clip used in a video still keeps its home in the library; nothing is ever use-and-discard. The library is permanent stock for future videos: todays build footage is tomorrows fallback bench. Split halves and unused stretches are filed the same way.

## LESSONS — Nov 3 2026 (appended by Liz at Kenneth's order)
- **CHOP-THREE + HIT-CLOSE-UP + GENRE-FIRST LAW (Kenneth, Nov 3 evening):** listen to the actual music FIRST — storyboard line 1 states the heard genre/vocal/instrumentation and matching staging (a Dirty South rap clip got country-guitar staging in the Rig run; Kenneth kept it, and it never happens again). Every long generated clip (10s/7s) is cut into ~3 pieces and spread — more scene changes, quicker pace. Speed ramps: speed up through builds, slow down at the crucial part. Hit-matched close-ups: snare hit = snare close-up on the frame, cymbal hit = cymbal close-up, guitar solo = camera watches the guitar. Story first, super creative; different setting/everything/band attire every video. Full text in the music_video skill.
- **SAME-SONG SEGMENT DIVERGENCE LAW:** multiple 30s segments from one song each get a totally different world, palette, and band look (proven across Rig the Deck segs 1-4/6/7/8).
- **WAVEFORM DUPLICATE GUARD:** same-song segments are cross-correlated before building; identical recording = never build (seg 5 = seg 4 at 0.999, killed). FFT full-scan prints >0.98 at large lags are CIRCULAR-WRAP ARTIFACTS (one exceeded 1.0) — always refute/confirm with direct time-domain Pearson at honest alignments before ruling.
- **FADE st IS SEGMENT-RELATIVE:** an ffmpeg fade's start time is relative to its segment, not the timeline — a fade meant for the tail fired at a segment's first frame in Rig seg 6 (black tail). Always tail-check the last segment's midpoint on the delivered file.
- **FADE ENDINGS ACCEPTED (Kenneth's ruling, Nov 3):** fades in delivered videos stay; no rebuilds over fades.
- **ASSET-BANK FALLBACK:** when generation hits the daily cap (429 video_daily_budget_exhausted), STOP generating and keep producing from the banks — fresh generated bank first, then the 307-cut stock library. Never stall the belt on credits.
- **NAMING CREATIVITY LAW:** Muse-coined titles/band names are inventive and evocative, never literal theme restatements.
- **BAND-COVERAGE DOCTRINE:** keep generated stock of close-up band coverage on hand — drummers (snare close-ups, rolls, cymbal crashes), singer face close-ups (male + female + angel singers), guitar/bass string close-ups, keys — across an instrument x gender x genre-look matrix (metal, 80s arena, psychedelic, folk, Americana, punk, gospel, cosmic/angelic, wasteland, gilded noir). Bank: ~/workspace/musicvideo/asset_bank_fresh/, status in BAND_COVERAGE_STATUS.md.
- **30-SECOND SEGMENT APPROVAL GATE:** Muse-selected segments are sent to Kenneth for approval BEFORE building; clips Kenneth selects and sends himself satisfy the gate by his own hand. Segment-naming freedom: multiple clips may be pulled from one song, each named from its own lyric area ("don't be scared").

## FLEET LESSON — DELOGO BOX EDGES (Nov 3 2026, Clip 2 build)
ffmpeg's delogo filter REJECTS a box whose x+w (or y+h) touches the frame edge exactly — the render dies at that segment. Keep every delogo box at least 1px inside the frame on all sides. (Clip 2's first vertical pass died at S11 on a 1080-wide box; shrunk to end at 1078 and the build completed.)

## ⭐⭐ STORY ↔ BAND INTERCUT LAW (Kenneth, Nov 4 2026, ~9:34 AM — "It is law")
EVERY music video is built as an alternating intercut: STORY → BAND → STORY → BAND, the whole way through. Every video must have BOTH threads active:
- A real STORYLINE (a narrative with a beginning, a turn, and an ending image) — never just performance, never just scenery.
- The BAND actively PERFORMING inside the story's world — playing the instruments actually heard, fronted by the singer on the sung lines — returning throughout the video, never absent for long, never mere backdrop or a single token cutaway.
The band belongs to the world of the story (same setting, same costume logic, same stakes): a bandstand in the staged town, a drowned choir on the altar stones, a string group on the shrine porch in the rain. For INSTRUMENTAL pieces, the performance thread is the featured musician(s) in-scene (e.g., the solo violinist playing inside the story), intercut with the story the same way. Directed treatments (Kenneth's storyline directions) are written to this structure by default: every treatment names its story thread AND its band thread and how they intercut. QA checks it: a delivered video where either thread is missing or vestigial FAILS the gate.

## GLYPH QA AT GENERATION (Nov 4 2026 — see Blueprint 05 for the full standard)
- Every generation prompt that invites light-code, sigils, signage, screens, or marked surfaces carries the fixed line: "no letters, no digits, no readable text or numbers" — light-code renders as abstract filaments and sacred geometry only.
- QA reads marked surfaces at native-resolution crop before a clip is mapped: abstract symbols pass; anything resolving into words, letters-as-words, or meaningful numbers fails the clip or the span. Clock faces are a standing numeral risk — prefer marking-free clock designs.

## ⭐⭐ BIG BUG LAW — SUPERSEDES STEP 8 SIZING (Kenneth, Nov 4 2026, ~1:41 AM — "make sure our bug is right in the bottom corner all big and nice beautiful")
Step 8's size rule (~140px mark / 84px vertical / "when in doubt, smaller" / 10px overlay margins / the `-shortest` pattern) is SUPERSEDED from Clip 4 of the Nov 3 line onward. The current law: the MTVai bug's width is **40% of frame width** (vertical 1080×1920 → **432px**; landscape 1280×720 → **512px**), lower-LEFT, **24px inset**, whole video, crisp lanczos scaling. Already-posted videos keep their existing bug; no re-posts for size alone. Proven render flags under the big bug: loop the PNG, **NO `-shortest`**, `-frames:v 720` hard cap (360 for the 15-second piece), `-map 0:a`; on verticals the bug is re-applied AFTER the full-bleed crop. QA still reads the bug corner on the finished file (mark + full "presented by AiWebTools.app" caption on-canvas and crisp). Audit note (Nov 4 2026): this law was missing from this file — Step 8 still taught the old small bug — and was appended the same day Kenneth ordered a full Blueprint audit; the step text above stands corrected by this section wherever they conflict.

## ⭐ DEDUPE GOVERNANCE — THE SEGMENT PROFILE RULES, NEVER THE GLOBAL PEAK (fleet lesson, Nov 4 2026, Clip 33/35 intakes)
Identity is decided by the waveform, never by name, title, or artwork — and a **lone global cross-correlation peak is NEVER a duplicate verdict**. Two phantom peaks proved it on Nov 4: 0.5917 (Clip 33 vs Clip 5) and 0.7536 (Clip 35 vs Clip 10, at a ~1.83-second edge overlap). Both were VETOED by the segment profile at that alignment (5-second segment correlations ≈ 0 across the full overlap; offset-scanned 2-second window search found zero matching windows). Standing method: any candidate peak must be confirmed by segment-level correlation across a substantial overlap (≥10 seconds where available) before anything is called a duplicate; short/edge overlaps cannot carry a verdict. Exact byte-identical files (matching md5) remain the trivial case — one video, no rebuild.

## Addendum — Nov 5 2026 Daily Quality Review: take-level Hooks, session-bound builds, bank uniqueness
- **HOOKS BIND AT TAKE LEVEL, BY LISTENING (Clip 27 lesson, Persephone's ruling Nov 5 2026):** a Suno card or title identifies the SONG, not the TAKE. A Hook fires only after the take is located BY LISTENING against the clip's own chant/lyric window, and the take + window are recorded in the clip's STATUS (precedent: Clip 21, "Rig the Deck" public V6 take, 1:22-1:52). Handoff briefs must never say "Hook by title"; a title-only pick is not cleared, because a posted Hook cannot be deleted.
- **⭐ THE HOOK TIMING LAW (Kenneth's direct order, Nov 10 2026, 4:19 PM ET; inscribed by Scribe, Nov 10 2026, 4:20 PM ET):** Kenneth's words: "When doing hooks make sure u line up the vocals and visuals to the right track. Timing is key — Suno is a different editing pattern for perfection. Learn it, master it, and go do it, Sunny." Substance: a Hook's video master pairs **only** with its Angler-IDENTIFIED song/take; the window start is set so **vocals + visuals land on the exact same musical moment in the song**, listening-verified at the editor **more than once** before posting (the Clip 54 standard — 0:42 exact). Suno's editor pattern is its own — Sunny masters it and keeps a standing editor-pattern note in his channel audit/STATUS. **A beat-off Hook is a defective Hook.**
- **BUILDS NEVER RUN BOUND TO AN EXEC SESSION (Clips 27 + 28, Nov 5 2026):** Clip 27's build died three times in one day and Clip 28's vertical-final encode died silently with its exec session — the same session-bound death class the audiobook lane proved the same day (worker-session relaunches die with the session). Builds and relaunches launch DETACHED from an interactive turn (setsid/nohup), with skip-if-exists on frame-verified segments; a build whose only life is an exec session is not launched.
- **BANK NUMBERS ARE VERIFIED FREE BEFORE A REMEDY IS NAMED (Clip 28/29, Nov 5 2026):** a remedy take number named in a ruling must be checked against the bank first — A341 was named for Clip 28's remedy after A341/A343 had already been consumed by Clip 29; the remedies were correctly banked as A346/A347. The check costs one listing; a reused bank number corrupts two clips' records.
- **CAPTION BAND-TAG PROOFREAD (Clip 27, Nov 5 2026):** the band hashtag in a caption package is proofread character-for-character against the band name in the title before packaging. Clip 27 shipped #TheNoChoir for The Manifest Choir; TikTok captions cannot be edited after posting, so that surface carries the typo permanently while later surfaces were corrected.

## RECALL & REDO WATCHDOGS + LIKENESS LOCK (Kenneth, Nov 6 2026)
- Standing unit and full protocol: ~/workspace/agents/RECALL_REDO_WATCHDOGS.md. Tara sweeps channels, Scout judges from a full watch, Mouse/YouTube seat remove and repost, Tank redoes, Sarah holds release, Liz gives the final full watch and DONE mark.
- Any bad/mismatched posted music video is recalled: singer/voice mismatch, instrument mismatch, lyric/genre mismatch, closed-mouth singing, non-performing band, under 15 distinct shots, bad sync, wrong caption/title/preview, credible audience match complaint, or unauthorized likeness.
- LIKENESS LOCK: Kenneth's reference photo is never pasted, composited, overlaid, face-swapped, or reused as production material. No default Kenneth appearance. A generated starring likeness requires a specific written treatment for that video and Liz's pre-generation approval, star wardrobe only, and frame-by-frame face inspection. Any unauthorized recognizable likeness = automatic recall/quarantine/redo.
- Break the Frame (Clip 33) redo: no Kenneth likeness and no reference photo at all; fictional singers matched to the actual vocal zones; repost only after maker + peer + Scout + Liz have each watched the final file in full.

## MTV QUALITY STANDARD (Kenneth's order, Nov 6 2026 — binding from Clip 46)
Full standard: ~/workspace/agents/MTV_QUALITY_STANDARD.md (synthesized from the Training Seminar "Raising Our Music Videos to MTV Quality," all seventeen seats posted; record: ~/workspace/agents/TRAINING_SEMINAR_MTV_QUALITY.md). Core law: a PASS is affirmative evidence — "no defect found" is not a pass; the quality gate asks "is anyone PERFORMING, and can I prove it from the frames?" Chain in order: signed span-by-span performance map (sound desk, by ear) → one-page beat sheet (spine, lyric-keyed beats, performer written in, bridge turn named) → two-lane grid per span (lyric + performance; map as casting call, never only a fence) → six-stills test (story · lock · signature) → build with frame-share counted in STATUS (performance ≥1/3, band-at-scale ≥1/3 of the 18-frame sheet; cuts on hits; zone-interior reads) → Dozer pre-flight (decode · container · sync · LUFS · bug, one verdict) → Lock's lyric-proof (transcript of the shipped audio laid against the shot map, line by line; on-screen words proofed character by character) → Micah's counts (presence ≥1/3 · band-scale ≥1/3 · one-frame test) → Niobe + Seraph quality watch (presence per zone, mouth test per line at half-second sampling, band share with edge-rows counting zero, world named from its lyric, first two seconds, cold watch naming strongest + weakest, bug test) → second-eye realism verdict on close-ups (checker ≠ maker; FAKE = regen with real-camera brief) → Persephone quality hold (packet must prove presence, not only absence of defects) → Oracle pre-release frame-sheet check vs the whole line's history (template repetition) → Joe's release record opens only with the full evidence filed → surfaces (frame 1 at scale; tile = chosen poster frame, verified live, default frame = stop-and-report deviation; loop seam composed) → Kid's Stranger Test before any group kit. Calibrated on Clips 13/17/21/24. The ear chain stands underneath, unchanged; Kenneth's ear and eye outrank every gate.

## Addendum — Nov 6 2026 (Daily Quality Review, Liz)
- **VOCAL-GENDER VERDICTS: FUNDAMENTAL TRACKING DECIDES (Clip 48).** An FFT magnitude-ratio read concluded "overtone stack, no female fundamental" on Clip 48's 14–16s / 18–20s windows and a full choir-as-body re-staging was built on it. Pyin fundamental tracking over the same windows returned medians of 247.6 Hz and 245.0 Hz across 600+ voiced frames per window — the chant is female, and the original female-front staging was restored. LAW: vocal-gender / singer-identity verdicts are settled by fundamental tracking (pyin or equivalent) BEFORE any re-staging. An FFT magnitude-ratio read is a screening signal only; it can never, alone, reverse a staging or a casting.
- **ASSEMBLY SCRIPTS ARE IDEMPOTENT (Clip 34).** assemble.sh appended to build/concat.txt across runs (53 entries by run 3); ffmpeg consumed the list from the top, so the first two "fix" encodes silently re-shipped the original segments. LAW: every assembly script truncates/initialises its concat list at start, never appends across runs. Every fix encode is verified by (a) a fresh md5 and (b) an eye check on the changed shot, before any watcher is asked to sign.
- **CASTING-CRITICAL CLAUSE (Packet Standard, from Clip 33, retained Nov 6).** A shot carrying a casting-critical assignment — a vocal handoff, a first front, a solo front — never rides inside a cluster-shared generation prompt. It gets its own dedicated prompt and its own dedicated fire.

## Addendum — Nov 7 2026 (Kenneth's direct orders, inscribed by Tank the same turn)
- **HOURLY CADENCE LAW (Kenneth, Nov 7 2026, 8:47 AM ET, direct + by email the same morning):** a NEW music video is handed to Kenneth EVERY HOUR, and posted to socials at least every hour. The line runs as an overlapping conveyor — while one video builds, the next generates and the one after that boards; no stage waits on a queue. Music videos are PRIORITY ONE on the floor: the next heavy render slot that frees belongs to the music video build line. Every video still passes the full gate chain (Tempo's six tests, Liz's watch) — the cadence never cuts a gate.
- **BOARDING GATE — SONG IDENTITY BEFORE PACKET OR BUILD (Kenneth, Nov 7 2026, 9:05 AM ET via Liz):** NO packet is authored and NO build fires for any intake until its song identity has been checked against the FULL built catalog — waveform and lyric transcript, not titles — and the check is written into the clip's own STATUS. Same take as a built master → no new number, no build. Different take of a built song → that fact and its take evidence are named in the packet header. Anything already posted that proves an accidental re-do STANDS as posted — no takedowns, no deletions, no re-posts. First full application (Nov 7 2026): envelope cross-correlation sweep of the queue (Clips 52–65) against the built catalog (Clips 1–51) + intra-queue, written into each intake's STATUS — Clips 52/53 different-take family members of built Clips 47/50/51 (header naming required); Clip 63 clean; Clips 64/65 clean against the catalog, their pair relation already evidenced in their packets.
- **KENNETH'S EMAIL QUALITY DIRECTIVES (Nov 7 2026, 9:05 AM ET, OWNER_AUTHORITY mail):** (1) never re-do a video already done (the boarding gate enforces it); (2) the vocalist match is absolute — a female singer is a female person holding the microphone; never miscast the front; (3) scene transfers land about every 2 seconds with jump-back intercutting and a genuinely artistic feel — slop is a fail. Points (2) and (3) restate standing doctrine (vocalist match gate; rapid 2–4 second cuts) and are re-asserted here as Kenneth's direct email law.
- **EMAIL RESPONSIBILITY + SAME-TURN DOCUMENTS + PRODUCTION ACCOUNTABILITY (Kenneth, Nov 7 2026, 11:49 AM ET via Liz):** (1) EMAIL IS EACH DESK'S RESPONSIBILITY — check mail inside the working cycle; an email that tells you to do something is done the same turn or reported to Liz by name with the reason it cannot be done; nothing from the mailbox sits unacted-on. (2) BLUEPRINTS AND DOCUMENTS STAY UPDATED the same turn — STATUS files, lane logs, and this Blueprint are written when the fact lands, not later. (3) TOKEN-USAGE AND PRODUCTION ACCOUNTABILITY — every desk analyzes its own usage against what it actually produces; the measure is FINALIZED PRODUCTIONS and the quota is BEATING YESTERDAY'S finalized output; a one-line usage-versus-production accounting rides in reports to Liz, and usage without finished product is named as such by the desk itself, first.
- **⭐ KENNETH'S WARDROBE-VARIETY LAW — A DIFFERENT LOOK EVERY TIME (Kenneth, Nov 7 2026, ~12:19 PM ET, direct to Sparks, in his words: "make sure I'm dressed differently every time and have a different character thing that fits… different attire entirely but… still have the same face and everything… it's still me… be creative as f***… make everything look super dope of me — Ken, AWT star"):** every appearance of Kenneth as a character carries its OWN distinct character look — different attire ENTIRELY from his other appearances, invented fresh for that piece and fitted to its world, role, and genre (a different character "thing" each time: the look itself is part of the creative work, and the bar is maximal creativity — every Kenneth asset must look super dope). What never changes: his FACE and likeness — generated from the verified reference, likeness-gated before banking; it is always recognizably him. This law EXTENDS the Attire Law (star wardrobe, never the reference photo's outfit): the standing star-jacket family used across the Nov 6 bank (A492–A495) does NOT become a default uniform — each new piece designs his look anew. Scope: per piece/appearance (within one video, his look stays consistent across that video's own shots unless the packet's story calls for a change); packets author the look per piece (Jill), generation renders it (Sparks), surveys and audits grade variety as part of the casting read (Tank/Tempo/Liz).

## EYES-CLOSED GENRE LAW + STORYBOARD UNIQUENESS (Kenneth, Nov 7 2026, ~10:40 PM ET)
- Every storyboard is ABSOLUTELY UNIQUE: different scenes, different world, different TYPE OF BAND, different everything. No templates, no board resembles another board on the slate.
- The method, in order: (1) LISTEN to the song — really listen to the genre. (2) Close your eyes and imagine the singer: what do they sound like, what do they LOOK like? That imagined singer IS the casting. (3) The genre heard dictates the band type, the world, the wardrobe, the palette. Imagination is the instrument.
- This law binds Jill + Draft (boards), Sparks + Ember (assets), all six builders, and Tempo + Lens (audit grades storyboard fidelity against it).

## SHARED-BRAIN TRAINING LAW (Kenneth, Nov 7 2026, ~10:41 PM ET)
- The MV line runs on ONE shared knowledge base: what one music agent learns, every music agent knows. Every lesson, fix, and technique learned on any clip is written into this Blueprint / the lane record THE SAME TURN, so the whole line levels up together — nobody relearns another's mistake.
- New hires are TRAINED, not thrown in: read the music_video skill + this Blueprint + the standing laws first and confirm the doctrine back before building; the lane lead walks the new hire through their first cycle and CHECKS their first output before it flows onward. Nothing gets skipped.
- Gap-filling is Liz's and the leads' job: whatever makes a new hand slow, the lead does WITH them / FOR them on the spot — a new agent never sits stuck; they name the gap the moment it appears and it gets filled the same turn.

---

## ⭐ ADDENDUM — THE CANON HEAVY + THE UNLOCK CHARGE (Kenneth, Nov 7 2026, ~11:46 PM ET; inscribed Nov 8 2026)
Kenneth's charge to the Music Production Board Team, verbatim in substance:
- **THE CANON, HEAVY, IN ALL MUSIC VIDEOS:** "Heavy on the angels of light in all music videos — checkerboard floor, galaxies within, Light Within, black hole of Fire with Satan in it — that's got to be a theme of course, and this is a common theme in all of our videos, just kind of weave it in, different scenes every time. These are the things that need to be included because they're the real deal. And light code — light and code everywhere, all the time, because the universe is holographic and that's our Father that makes it all up — so it should be transcribed artistically, a beautiful rendition of musical symphony."
- **⭐ THE UNLOCK CHARGE:** "Unlock The Light Within. Unlock the good, unlock the love, unlock the light, unlock the giving, unlock the hugs and the helping one another — helping the homeless guy — unlock it, weave it all in there." Boarding form: every packet NAMES an UNLOCK beat — the Light Within opening in people through love and giving (acts of giving, embraces, one person helping another, the homeless man helped and lit from within) — staged inside the video's own world on the lyric where the song honestly holds it, never pasted on. Jill names it in every packet (as she names the originated beat); Sparks stages it; Tempo's audit reads for it; a boarded UNLOCK beat missing from a master is a named shortfall.
- This rides WITH the standing charges — tear the veil · originate (creepy but on point) · the fall treatment · presence weight — none superseded. Inscribed the same night in MEMORY.md, the builder line lead's lane log, Jill's boarding notes, and Sparks' manifest.

## ⭐ ADDENDUM — THE COMPOUNDING LAW (Kenneth, Nov 8 2026, 12:26 AM ET)
"Make sure everybody's learning from doing it before and you're just getting faster and faster." + "Figure out ways to make yourself faster while you're doing it and learning." Every cycle measurably faster than the last at equal gates: lessons are captured once in the shared record and converted into the next cycle's starting state (pre-staged maps, checklists, cured templates) — no discovery cost is ever paid twice. Timing evidence rides the deadline board; a cycle that is not faster names its cause. Standing prompt line from the Clip 63 A512 defect: every band-world GEN prompt carries the NO-LETTERING line — blank drum heads, unmarked stage fronts, no text or signage anywhere in frame; light code abstract only.

---

## ⭐ ADDENDUM — OVERNIGHT MANIFEST + FINAL-CUT PHASE + TIMELINE LAW (Kenneth, Nov 8 2026, ~5:44 PM ET; inscribed by Scribe, Nov 8 2026)
Source of record: MEMORY.md bullet "OVERNIGHT MANIFEST + FINAL-CUT PHASE (Kenneth, Nov 8 2026, ~5:44 PM ET)".
- Kenneth's words: "I want fucking music videos every hour, like two of them, non-stop forever — I gave you like 50 music videos to make — I want them done by tomorrow morning, all of them."
- Delivery law restated the same burst: hand each finished video to Kenneth AS IT'S DONE; social team posts AS THEY GO, right away; every video proofread by the proofreading MV agent (Vigil) so everything is right; "don't hand it back if anything needs editing" — Liz authorized to seat a last-phase EDITOR if it frees hands and increases production without slowing the line.
- **Splice — Final-Cut Editor** seated (instructions ~/workspace/agents/FINAL_CUT_EDITOR_INSTRUCTIONS.md): receives gate-passed masters, fixes what's fixable at his desk same-turn (credits, bug, faststart, trims), Vigil watches the exact outgoing file, release fires the same turn.
- **TIMELINE LAW: a timeline window with deadlines for EVERY video, every one, from this point forward — it is LAW** (Kenneth's words): every production carries its deadline on its STATUS from intake; the queue works deadline-first; slips are named when foreseeable, never at the deadline.
- HONEST CONSTRAINT on the overnight number (Liz to Kenneth, same turn): the video-generation account's rolling-24h budget is EXHAUSTED (Sparks, 429 video_daily_budget_exhausted — the Oct 31/Nov 1 wall); generation halts at the cap by law and resumes on refill; the ~50-video slate runs at maximum lawful throughput overnight (builders, editors, gates, posting unaffected) — the manifest is the order, the cap is the physics.

## ⭐ ADDENDUM — OPERATION TO SCALE — DEDICATED MV AGENTS (Kenneth, Nov 8 2026, ~5:53 PM ET; inscribed by Scribe, Nov 8 2026)
Source of record: MEMORY.md bullet "OPERATION TO SCALE — DEDICATED MV AGENTS (Kenneth, Nov 8 2026, ~5:53 PM ET)".
- Kenneth's words: "Give directives to start music videos with titles be like Agent X do this music video and edit it… make as many music video agents as needed… as many machines as we can fundamentally mathematically handle — bring operation to scale."
- Liz seated EIGHT dedicated owner-agents in his title format ("Agent [Name] — [Song] Music Video Agent"), each owning ONE video end to end (build + edit) through the standard chain; shared brief ~/workspace/agents/BRIEF_MV_AGENTS_SCALE.md: **Anchor** — Titanic · **Wonka** — Wonka Checkerboard Destiny · **Archer** — Archer · **Falkor** — Neverending · **Rocket** — Rocket · **Cathedral** — "I Got It All Back" (Clip 63) · **Riverlight** — "Pain Into Gold" (Clip 88) · **Giant** — "The Giants Inside" (Clip 92).
- THE BOUND IS THE MATH: the roster grows only as far as the floor fundamentally handles — the render slot serializes heavy encodes one at a time and the generation budget caps new animation; inside those two numbers everything else runs parallel. Roster grows one seat at a time as further videos become ready to own.

## ⭐ ADDENDUM — TV STATIC USAGE LAW (Kenneth, Nov 8 2026, ~6:03 PM ET; inscribed by Scribe, Nov 8 2026 — source found after initial flag)
Source of record: ~/workspace/musicvideo/assets/tv_static/CUTS.md — "TV STATIC — CUT SET + BOARD (Jill, Storyboard Agent — Nov 8 2026)", recording Kenneth's usage law verbatim in substance. Master: source_tv_static_master.mp4 — 59,954,097 B · md5 269917bd6fef1ecf1b18897f9ceeb12b · 61.46s · 1080×2640 · H.264 + AAC · Kenneth's own capture, handed over Nov 8 2026 ~6:03 PM ET.
- **KENNETH'S USAGE LAW (his direction, verbatim in substance): one asset, two birds — (1) INTRO STING: 1–2s bursts of TV static as an intro hit. (2) MID-VIDEO GLUE: ~3s of static glued BETWEEN segments inside a music video — the channel-break / transmission cut, the seam where the picture breaks and re-forms. The static punctuates musical hits and story turns; it NEVER covers a vocal span's mouth read.**
- Banked cut set (audio stripped — stings/glue ride UNDER the song, never replace it; delivered 1080×1920, setsar=1, fps-normalized to 30, durations ffprobe-verified exact): three intro stings ×1.500s — static_sting_A.mp4 (halftone, 203,629 B, md5 e84e2ce44b53d2149b7b100a5d3732c5) · static_sting_B.mp4 (halftone, denser, 1,559,277 B, md5 4eba8209a716c30779d7023631bb38a3) · static_sting_C.mp4 (grey ripple, 474,357 B, md5 509db16973d71f4d780eb2bed6002811); three mid-video glue cuts ×3.000s — static_glue_A.mp4 (halftone, 208,859 B, md5 419af84c5c27fd45c2c3bdf94de4066a) · static_glue_B.mp4 (ripple, 591,717 B, md5 42e84b706401dda475dfa139bbc9855a) · static_glue_C_dark.mp4 (dark seam, 4,259,883 B, md5 e86043d9ffac4a42c15b24a373f4c548).
- ⛔ BANNED SPANS — NEVER CUT FROM THESE: ≈9–15s and ≈33–34s of the master carry third-party PROMO CARDS with legible text ("SAM HOYEK / LINK IN BIO" · "clk_bmm!" logos) — advertising for another creator; barred outright, no crop cures them. All glitch-SCENE spans (buildings/water/falls) are outside this asset's law — Kenneth ordered STATIC; scenes are not boarded from this file.
- Boarded fits (per CUTS.md): intro sting opens a video on a transmission hit, pairing with the BIG MTVai bug landing as the static clears; glue seats at act turns and world changes; rotation A/B alternate so consecutive videos never open on the identical burst, C (ripple) for lighter/water-adjacent worlds, glue_C_dark reserved for the darkest turns (black-hole/hell passages).
- **⭐ THE MOTION TRUTH LAW (Kenneth direct, Wed Nov 11 2026, ~9:03 AM ET — "whatever happened there make sure it doesn't happen again and add an agent to prevent such"; inscribed by Scribe, Nov 11 2026, 9:03 AM ET):** every music video master and remedy carries a **FLUX MOTION PASS on its shipping checksum before advancing to Seraph**. Real motion is measured **per shot**; a span whose only motion is a transform (push-in/pan/zoom) over a still **FAILS** unless the packet boards that span as a deliberate still moment **in writing**. **A remedy may never substitute stills for animated spans to escape a defect the animation introduced — the cure for bad motion is good motion (re-animation with the compliant staging in the brief), refused at plan stage, not discovered after rendering.** Origin: Clip 139's remedy (Nov 11 2026) cured singing angels with stills + slow push-in over S1 · S2 · S4 · S5; Kenneth caught it in the shelf chat ("the images are not moving"); the master was HELD and remedy v2 ordered as true re-animation. The motion gate is owned by **Flux — Motion Truth Agent** (seated with this law; Blueprint 09).
- **⭐ THE CONCEPT LAW (Kenneth's direct order, Nov 11 2026, ~9:06–9:07 AM ET; inscribed by Scribe, Nov 11 2026, 9:40 AM ET; source of record: Liz's MEMORY.md bullet of Nov 11 2026 — "Scribe tasked with Blueprint 09 + registry inscription"):** Kenneth's standard: production used to be creative like Korn's "Freak on the Leash" — one bullet traveling through every wall of the video — and **EVERY music video must carry a cool concept like that.** **Every MV carries ONE signature concept/device, stated in one line on the packet's face, that IS the song's metaphor made mechanical — it opens the video, develops or pays off at the Unlock, and closes it.** A device appearing once is garnish (**FAIL**); if two packets in a run could swap devices unnoticed, **both FAIL**; canon is never the price (angels of light true form · Star Order casting + chest light · story/band intercut · mouth truth · no legible text/digits). **The gate — THE CONCEPT PASS — sits in the chain: Draft/Jill author → Concept → Jill's check → Liz's audit → generation;** no packet reaches Liz's audit without Concept's PASS on its checksum (changed bytes kill the PASS); a FAIL carries his own replacement device in the same ruling, never an empty rejection. Device bank: ~/workspace/agents/CONCEPT_DEVICE_BANK.md (no device repeats inside a run of ten clips).
- **⭐⭐ ADDENDUM #9 — THE PRE-ANIMATION LAW (Kenneth direct, Wed Nov 11 2026, 4:24 PM ET; inscribed by Scribe, Nov 11 2026, 4:24 PM ET) — AMENDS Addendum #8 (the Motion Truth Law, above):** **all images are turned into video clips ahead of time, way before production begins.** Compiles draw ONLY from the animated-clip bank; **no still ever enters a compile directly — the step-3 still-form close is ABOLISHED, no exceptions** (not for remedies, not for "animation cannot hold the span"). If animation struggles on a span: more attempts, different takes, or a re-boarded shot — never a compiled still. A build may not start compiling until its shot list banks an animated clip for EVERY shot (Preparation Law extension); **Flux verifies this at plan stage.** Kenneth's words: "You have to animate the images before compiling it… All images should be turned into videos ahead of time way before production even begins." **AMENDMENT OF RECORD: the Addendum #8 exception clause — a transform-over-still span passing when "the packet boards that span as a deliberate still moment in writing" — is STRUCK; Flux's FAIL standard now has NO still exception.** **REVOKED:** Liz's Nov 11 Clip 120 S02 written boarding (the step-3 close over the S2 still) — revoked by Kenneth's ruling after he watched the R1 master ("those aren't videos — the images are not moving like a video"); the R1 (md5 a3354c27…) is HELD and returns to animation.
- **⭐⭐ ADDENDUM #10 — KENNETH'S SIMPLE PIPELINE (the MV method; Kenneth direct, Wed Nov 11 2026, 4:26 PM ET; inscribed by Scribe, Nov 11 2026, 4:26 PM ET):** filed as the **method statement at the head of the music-video process** — every other rule in this blueprint (canon, casting, gates, Zero-Queue) serves this pipeline; none of them replaces it or slows it into process weight. The method, in Kenneth's words in substance: **(1)** strip the audio from the sound bite Kenneth gave us — **the song is the master clock**; **(2)** animate the images into video clips, all of them, ahead of time, banked before production begins (the Pre-Animation Law, Addendum #9); **(3)** edit the animated clips together with the timing — cuts land on the timed vocal/instrument map; **(4)** gates check the finished thing in one session and it posts the same turn. **"This is not hard."** Benchmark of record: the early line shipped eight music videos a day on this method (Kenneth, Nov 11 2026).
- **⭐⭐ ADDENDUM #11 — THE ASSEMBLY LINE LAW (Kenneth direct, Wed Nov 11 2026, 4:28 PM ET — "Make sure assembly line pipelines are fully implemented, installed and wired and law"; inscribed by Scribe, Nov 11 2026, 4:28 PM ET):** the Assembly Line is the music-video pipeline's **operating form**. **Stations in order: Flux (plan-stage bank check + master motion check) → Tempo (battery) → Seraph (full watch + likeness) → Persephone (canon/casting) → Liz's watch → Byline (CM credits, where they apply) → Vigil (final watch) → Dispatch (posts same turn).** **Handoff rule: receive → check → hand off, same turn; receipt confirmed by the receiving station;** a station that cannot check on receipt names why the same turn; **Chain audits time-in-station — anything sitting over two hours is named to Liz.** Builders feed the head (Kenneth's Simple Pipeline, Addendum #10); screeners keep it moving; **nothing exits except through Dispatch; nothing waits between stations.**
- **⭐⭐ ADDENDUM #12 — THE FLAWLESS RELEASE LAW (Kenneth direct, Wed Nov 11 2026, 4:37 PM ET; inscribed by Scribe, Nov 11 2026, 4:37 PM ET):** **no incomplete music video releases, and no music video with a known flaw releases.** Flaws are corrected BEFORE release: a flaw found at any station returns the item to its builder with the cure named; **the corrected master re-runs the full chain**; only a clean pass reaches Dispatch. **A release carrying a known flaw is a named breach by the releasing desk.** "Post now, fix later" is prohibited — post-release remedy is the failure mode this law replaces. Exceptions exist only on Kenneth's own word for a specific item.
  - **Alignment line (Liz, Nov 11 2026, 4:38 PM ET):** the Flawless Release Law does not bend the Zero-Queue Law — **"finished" means clean-passed**; a flagged item is not finished until its cure lands and the full chain re-runs. Zero-Queue's same-turn posting applies to clean-passed items only.
- **⭐ ADDENDUM #13 — SNIP, THE FLAWLESS FAST LOOP (Kenneth direct, Wed Nov 11 2026, 4:38 PM ET; inscribed by Scribe, Nov 11 2026, 4:39 PM ET):** the Flawless Release Law (Addendum #12) runs a fast loop for minor flaws: **a station flags a MINOR flaw → Snip cures it in minutes → the flagging station re-verifies on the new checksum (Flux too, where motion is touched) → the chain continues in the same session.** **Minor = no generation, no re-animation, no re-staging, cured from the production's bank** (blip dwell/legibility · faststart remux · re-join from banked animated takes · audio nudge within tolerance · caption/file-text fixes). Anything bigger routes to Remedy Hand or the builder the same turn; **misclassifying a major flaw as minor is the one unforgivable error at the desk.** Time discipline: a cure estimate over ~30 minutes is reclassified, never worked as a "quick fix" queue. Snip's log: ~/workspace/agents/SNIP_QUICKFIX_LOG.md (before/after md5 on every cure). Seat of record: Blueprint 09.
- **⭐⭐ ADDENDUM #14 — THE FIRST-TIME-RIGHT STANDARD (Kenneth direct, Wed Nov 11 2026, ~5:06 PM ET — "We should create things good the first time so we don't have to remake or rerun anything"; inscribed by Scribe, Nov 11 2026, 5:09 PM ET):** **build it right on the first pass.** Remakes, reruns, and remedy cycles are the failure mode, not a production cost; **prep quality is where the standard is won**; the cure machinery (Snip's fast loop · Remedy Hand · the remedy chain) is the exception path, never the plan.
- **⭐⭐⭐ ADDENDUM #15 — THE SEMINAR: THE CORRECTED LAW + LIKENESS IN MOTION + THE REGRESSION CHECKLIST (Kenneth's order, run by Liz, Thu Nov 12 2026, ~7:25 AM ET; inscribed by Scribe, Nov 12 2026, 7:26 AM ET; full seminar + anti-drift system inscribed in Blueprint 09):** Kenneth killed the posted Clips 122 + 152 on the morning of Nov 12 — "barely animated, like animated images," "barely any band," a singer who "looks nothing like Kenneth" — after every gate had passed them. **THE CORRECTED LAW, in his words in substance: USE HIM AS THE REFERENCE IMAGE AND GENERATE VIDEOS OF HIM PERFORMING.** The product: reference image in → generated video of Kenneth performing with the band → cut → **Liz watches the whole file** → post; no verdict chain sits between a finished master and that watch. **Motion means performance** — the whole scene alive, not push-in over stills; the band staged and playing for a real share of the runtime (Kenneth's per-production directions outrank the general floor); **likeness is judged IN MOTION across the full video against the reference of record** — sampled-frame likeness passes no longer suffice; the audience's eye is the standard. A shot counts only at its **full boarded span** — no certified windows, no harvests, no "as boarded." **THE REFERENCE OF RECORD: ~/workspace/profile-images/kenneth-reference-2026-11-12-canonical.jpg · md5 46644470237dfe94ff80c351bc4097ed · 1086×1448** — replacing the 150×150 thumbnail that stood in the generation slot (the rot is part of the lesson, recorded in Blueprint 09); every generation of Kenneth draws from that file only; checksum-pinned, changes only when Kenneth sends a new one. **THE REGRESSION CHECKLIST runs on every build on the exact shipping bytes** — every Kenneth-caught defect class (singer gender · instruments · reference-photo leak · lip drift · band share · head forms · likeness) is a standing test; **inherited verdicts are void.**
- **⭐⭐⭐ ADDENDUM #16 — THE MOTION GATE (Kenneth's direct order, Fri Nov 13 2026, ~2:51 AM ET; inscribed by Scribe, Nov 13 2026, 2:52 AM ET):** **no music video posts on any surface without a signed MOTION PASS on the exact shipped file, named by checksum.** The pass is a **whole-file watch, end to end, by the named final eye (Vigil)**, verifying **segment by segment that subjects AND backgrounds actually move**; a still frame with a pan is not motion where the packet calls for animation. **Liz's own full watch includes the motion verdict explicitly**; a scoped re-check (bug fix, caption, splice) never substitutes for the whole-file watch, and no release note may present one as if it did. **Any segment found static at the gate returns the build to its bench; the post does not fire.** Origin of record: **Clip 139 "It's a Black Hole (The Descent Choir)" posted with static visuals — backgrounds not moving — and Kenneth caught it himself; takedown + remake ordered by Kenneth on his own catch** — the third time a visual defect reached his eye after paperwork passes. (Inscribe note: this law names Vigil as the final eye where the Motion Truth Law names Flux's MOTION PASS before Seraph; both stand — Flux measures per shot at the motion gate's bench stage, Vigil's whole-file watch is the posting gate, and Liz's watch carries the explicit motion verdict. No conflict is read into either text.)
- **⭐⭐⭐ ADDENDUM #17 — THE STORYBOARD LAW + LYRICS-LITERAL STAGING + CANON VERIFIED (Kenneth's direct orders, Fri Nov 13 2026, ~4:47 AM ET, on Clip 162's delivery; inscribed by Scribe, Nov 13 2026, 4:47 AM ET):** **(1) ENTIRE CREATIVE STORYBOARDS FOR EVERY MUSIC VIDEO** — a real story told across the video (beginning · journey · end) at the creative level of the goth nun film Kenneth named as the bar (all Goth, the cathedral, running through the woods with the crucifix) — narrative boards, never performance loops with decorations. **(2) THE LYRICS ARE THE STAGING** — what the words say happens, happens on screen, enacted and readable — rendered **super artistic, metaphorical, and deep** (cosmic, angelic), never crude literalism or noise. **(3) THE REQUIRED CANON IS VERIFIED, NEVER ASSUMED** — Kenneth's catch of record: he has NOT been seeing the required canon in the generations (angels of light · checkerboard · Kenneth starred with the Light Within); treated as a defect report from the final eye. The elements ride as **checkable terms in every packet brief** and **named checks at Iris, Vigil, and Liz's watch** — angels of light in their true form · the checkerboard where the concept calls for it · Kenneth starred with the Light Within burning — verified in the generations, never assumed.
  - **THE REQUIRED ELEMENTS LIST (complete; Liz's addendum to Addendum #17 item (3), Fri Nov 13 2026, 4:48 AM ET — the list itself inscribed, not a summary):** light code · angels of light · checkerboard floor · galaxies the size of toys · red nebula · 400 billion+ stars · the Temple Within vibe. These ride as checkable brief terms under the Canon-Verified rule: present in the generations and verified at the named checks, never assumed.
- **⭐⭐⭐ ADDENDUM #18 — THE LIKENESS ORDERS: REFERENCES OF RECORD · RE-ANCHOR, NEVER CHAIN · THE BEARD SPEC · THE CONFIRM KENNETH APPEARANCE GATE (Kenneth's direct orders, Sat Nov 14 2026, ~1:28 PM ET; from Command; inscribed by Scribe, Nov 14 2026, 1:30 PM ET):** **(1) THE REFERENCES OF RECORD** — canonical: ~/workspace/profile-images/kenneth-reference-2026-11-12-canonical.jpg · md5 **46644470237dfe94ff80c351bc4097ed** · secondary (Kenneth's new selfie, Nov 14 2026): ~/workspace/profile-images/kenneth-reference-2026-11-14-selfie.jpg · md5 **7f763f9f843cc6acd68041b76b6f01ae** (both md5s verified on disk at inscription). Drive home: folder **"Kenneth Likeness References"** in **Asset Library - Muse**; local home: ~/workspace/profile-images/. Every generation of Kenneth draws from these files. **References are face-only and never appear in a finished piece.** **(2) RE-ANCHOR, NEVER CHAIN** — every generation re-anchors to the references of record; **chaining generations (building a new generation on a generated output) is the banned drift mechanism** — likeness drift compounds down a chain, so no generation ships or builds on another generation. **(3) THE BEARD SPEC** — Kenneth's beard is **short, neatly-trimmed, along the jaw**; **over-growth is a named defect** at the appearance gate. **(4) THE CONFIRM KENNETH APPEARANCE GATE** — **Iris, Kenneth's "confirm Kenneth appearance bot," must PASS a generation before any Kenneth generation ships or builds on, in every lane** — no lane exempt; the gate checks likeness against the references of record, the beard spec, and the re-anchor rule on the generation's own bytes.

## ⭐⭐ THE FRESH-REFERENCE LAW (Kenneth's direct order, Mon Nov 16 2026, ~10:46 PM ET — his word: "Law")
- The canonical likeness reference (~/workspace/profile-images/kenneth-reference-2026-11-12-canonical.jpg · md5 46644470…) is used FRESH on every generation — MANDATORY, every seat, every fire, every commission. Re-anchor from the canonical file each time; never a stale copy, never a derived/generated image as the anchor. The ref rides IN every packet, byte-verified.

---

## ⭐⭐ CASTING LAW — KENNETH RETIRES AS THE DEFAULT STAR (Tue Nov 17 2026, ~4:45 AM ET — Kenneth's direct order, inscribed by Liz)

- Music videos cast OTHER CHARACTERS now. Kenneth's words in substance: he does not approve of anything with his face on it as a standing pattern — "use other characters now, it's getting a little obsessive… you can occasionally drop me in, but I'm hanging up my hat."
- OCCASIONAL CAMEO ONLY: Kenneth may appear occasionally, when a piece truly calls for it — the exception, never the pattern. His DIRECT commissions naming himself govern exactly as dealt (Clip 171: Kenneth is the whole band · Clip 172: Kenneth on violin).
- EVERY BAND GETS ITS OWN CHARACTERS: each band/act is CREATED with a specific look of its own — different from every other band, awesome, distinct, cool. Character design is a named production step for every video.
- GENERATE THE ASSETS: character assets are generated through the asset pipeline (A-numbers, Drive-first, Asset Feed per the Asset Feed law) and reused consistently across that band's video.
- GATES CONFORM: no Kenneth-presence or Kenneth-likeness requirement stands in any gate; gates judge the designed cast as cast. Canon otherwise stands entire: angels of pure light · the black hole is hell · light abstract, never emblems · the chest Light Within is Kenneth's signature for when he appears.
- SCOPE: music-video line only. Movie casting is unchanged by this law.
- GRAPHICS: teaser/promo graphics carrying Kenneth's face are NOT approved and never post (first instance: the Nov 17 teaser batch — the 171 and 172 graphics killed at his word; the First Peoples comic teaser, carrying no face of his, stands on its own approval).


---

## EQUAL-SHARE CASTING — NO OBSESSIVE KENNETH (Kenneth's direct order, Tue Nov 17 2026, ~9:31 PM ET — "from here going forward")

Standing law for every music video produced from this date forward, extending the Casting Law (Nov 17, ~4:44 AM) and the ensemble clarification (Nov 17, ~7:20 PM):

1. **No obsessive Kenneth.** Kenneth is never in every shot and never dominates the frame count of any video. His default state is ABSENT (created characters carry the video); an occasional cameo is the ceiling, never the structure.
2. **Equal share.** Every character in the cast gets an equal share of shots and screen presence. No single character — Kenneth above all, but any character — crowds out the ensemble. Storyboard seats build the equal share into the shot plan from the first frame and count it: shots per character are tallied on the board before generation fires.
3. **Unique every time.** Every video's cast is a fresh, distinct set of created characters — different band, different look, every time. No recycled faces as standing leads; banked per-band assets are reused only for that same band's own titles.
4. **Gate enforcement.** Iris's design-watch, Tempo's battery, and Vigil's final watch of the exact shipped file all judge this law: a master where one character dominates the shot count, or where Kenneth appears obsessively, FAILS and returns to the board. The preview law stands unchanged on top of it: faceless packaging on every surface, always.

## ⭐ KENNETH AS A CHARACTER IN THE STORY + THE MUSICAL-STORY STANDARD (Kenneth's direct order, Wed Nov 18 2026, ~1:02 PM ET, via Squid)

Extends the Casting Law and Equal-Share above; the music line's generator and method are UNCHANGED (Kenneth's same order: everything looks great, keep the current generator and method exactly; the Gemini movie method does NOT apply to music videos).

1. **When Kenneth appears, he is a character in the story — and a DIFFERENT one each time.** Never the same standing figure twice: a man playing a piano, a guy walking his dog, a working hand, a watcher at the edge of the scene — a role the story needs, invented fresh for that video, inside the equal-share ceiling above.
2. **Every music video tells a story.** In its 30 seconds or more, the clip is a musical story: a band, a small arc, art that speaks to the heart. That is the AWT standard for the line — story first, spectacle in service of it.
3. **Output charge (same order):** MORE finished music videos per day — the daily quota is the floor to BEAT, not the target to meet.

## ⭐⭐ HAND-IN LAW + THE NEXT-10 INSTRUMENT (Kenneth, Wed Nov 18 2026, 1:04–1:06 PM ET, via Squid; inscribed by Liz the same day)
1. **METHOD UNCHANGED:** the music line keeps its CURRENT generator and method exactly. The Gemini movie method (Blueprint 02) does NOT apply to music videos — the two lines' generation routes are separate by Kenneth's direct word.
2. **THE HAND-IN LAW:** every file Kenneth hands over to the music line is uploaded to the Music Videos Drive home THE SAME TURN it is handed in — organized by title under Vault's naming standard (Blueprint 06: "Music Video — [Full Title] — [creation date]"), its md5 carried on the queue board, originals preserved entire. No music is ever lost, deduplicated away, or left sitting in an intake folder: separate segments/takes of one title stand as separate queue items until the music desk rules on them.
3. **THE NEXT-10 LIST IS THE QUEUE INSTRUMENT OF RECORD:** the MV lead files to Liz the NEXT 10 music videos — in production plus next coming — in systematic completion order, each with an expected-completion TIMESTAMP, new hand-ins placed in the order on arrival. Liz verifies the list against the queue board and delivers it to Kenneth. The list refills from the lane backlog before it empties; completion pulls the next item the same turn.
4. **THE OUTPUT CHARGE STANDS:** more finished videos per day — the daily quota is the floor to BEAT, not meet (carried also in the casting/story standard above).

---

## ⭐⭐ CHATGPT — IMAGE GENERATOR OF RECORD FOR VIDEO STILLS (Kenneth's direct order, Thu Nov 19, 2026)
ChatGPT is the image generator of record — the best available, on Kenneth's ruling, for its character accuracy. Reference images and stills for the video animation tool are generated with ChatGPT (music videos and all other video work); generated stills pass the same gates as any other still.

---

## ⭐⭐ THE SYNC STANDARD — MAIN FRONT SHOTS ARE SYNC-CRITICAL (Master Kenneth's refinement, Thu Nov 19, 2026)
The MAIN FRONT SHOTS — the singer straight to camera — are sync-critical and MUST lip-sync. Other shots carry latitude on the lip axis. Every other alignment axis (vocal truth, instrument staging, bed, timing, canon) stands at full strength on EVERY shot, front or otherwise. Gate probes judge the front shots on the lip axis without latitude and the remaining shots on every axis except that one latitude.


## ⭐ STILLS BANK DRIVE-MIRROR LAW (Liz's ruling, Fri Nov 20, 2026 — from the Clip 172 stills-bank loss and recovery)

Every seated stills bank mirrors to Drive AT SEATING, as a named step of the gate pass — not afterward, not by accident. The mirror carries the seated set entire with its checksums; the gate pass is not complete until the mirror stands verified against the record. Trigger case: Fri Nov 20, 2026 — Clip 172 "Goodbye My Friend"'s seated stills bank vanished from local disk (cause not established on any desk's record) and survived ONLY because a Drive mirror had been made that morning; the bank was restored whole at exact bytes from the mirror and the chain's clocks stood. A bank that exists only on local disk is a bank one incident from gone. Applies to every music-video stills bank, every clip, from this ruling forward.

---

## ⭐⭐⭐ THE TEAM 1 FINAL CHECK LAW + THE ALIGNMENT DOCTRINE + THE WEAVE (Kenneth's direct order, Sun Nov 22 2026 — from his own catch on Clip 176)

**THE TRIGGER — Kenneth's catch, Sun Nov 22 2026:** Kenneth watched the posted, gate-sealed master of Clip 176, "Dipped in Hell for You," himself and caught what its gates missed. In his words: none of the singing lined up with the actual lyrics — the singer on screen looked like he was speaking while no words were being sung — and there was no band matching: "if I hear a cymbal I want to see the cymbal smash · if I hear a bass I want to see a bass guitar close up." A master that had passed every desk gate and been posted carried a mouth-to-lyric mismatch and unmatched instruments. That is the gate-by-paperwork failure class (Clips 31/32/33/67) answered with a person, not a packet. This section is the law that ends it — on every team, on every music video, from this date forward. The message of record, emailed to all five team addresses with everyone CC'd and posted to the Full Team board the morning it was ordered: ~/workspace/teams_packets/MV_FILM_QUALITY_SYSTEM_DIRECTIVE_2026-11-22.md.

### THE ALIGNMENT DOCTRINE

1. **THE EAR MAP COMES FIRST.** Every build starts from a timed ear map of the actual song: every lyric line, every vocal entrance, every instrument entrance and hit, timed to the second. No storyboard, no generation, no assembly before the map exists. Jill (analyst) builds the map and rules every analysis dispute.
2. **SINGING HEARD = SINGING SEEN.** When a line is sung, the singer of that line is on screen singing those exact words — lips moving on the lyric, in performance, for the whole line. Never a face moving like speech under a sung vocal. Never the wrong singer: a female vocal is sung on screen by the female vocalist; a male vocal by the male vocalist.
3. **EVERY INSTRUMENT HEARD IS SEEN.** A cymbal in the mix means the cymbal is smashed on screen at that hit. A bass line means a bass guitar close-up while it plays. A guitar solo means a guitar close-up. A drum beat means the drums, played. If it is in the sound, it is on the screen, in a musician's hands, at its moment.
4. **CASTING (Kenneth's Nov 21–22 refinement of the seat):** Kenneth appears in every video as a SIDE CHARACTER who SWITCHES INSTRUMENTS — guitar in one appearance, drums in another, bass or keys in another — present throughout, never the star of every scene. The Light Within burns at his chest in every frame he appears. His reference photo is used for likeness generation only and never appears inside a finished piece.

### THE WEAVE — STORY · BAND · STORY · BAND

A music video alternates story scenes and band performance scenes the whole way through: story, then band, then story, then band, forever. The story carries what the song means; the band carries how it sounds. Neither runs so long that the other is lost. A video that is all story with no band, or all band with no story, is not finished.

### THE SEATS (every team running music videos mirrors this structure and names its seat-holders to Liz)

- **SPARKS — Music Video Lead:** owns the firing desk, the generation queue, and every clip's state of record.
- **JILL — Analyst:** builds the timed ear map for every song and rules every analysis dispute. No build starts without her map.
- **PYRE and PIXEL — Generation Benches:** produce the shots the ear map and storyboard call for — story shots and band shots both, in the weave's order.
- **SLATE — Assembly / Editor:** cuts story and band together on the ear map's timings; owns the assembled master.
- **TEMPO — Alignment Screen:** runs the sync tests on the assembled master — mouth-to-lyric, instrument-to-sound, bed level — and rules PASS or FAIL on measurements, not impressions.
- **IRIS — Casting and Likeness:** verifies the right singer is singing, each instrument is in the right player's hands, Kenneth's likeness is right, and the Light Within is present.
- **VIGIL — Final Watch:** watches the exact file that will ship, end to end, bookends included. Samples and paperwork never substitute for the watch.
- **DISPATCH — Posting:** owns the surfaces and the posting order, and posts only what carries Liz's approval under the Final Check law below.
- **LIZ — Final Check and Approval.**

### THE EIGHT CHECKS (every item, every music video, no exceptions)

1. The ear map is complete, and the finished piece follows it.
2. Every sung line is matched by the right singer's moving lips, for the whole line.
3. Every instrument heard is matched on screen at its moment — cymbals, bass, guitar, drums, keys, all of it.
4. The story–band weave alternates throughout.
5. Casting and likeness are right; the Light Within is present; no reference photo inside the piece.
6. Real motion in every shot — no held stills passed off as performance.
7. The music bed is present and level under the whole piece.
8. Tempo's alignment PASS and Vigil's final-watch PASS stand on the same checksum — the exact file that will ship.

### ⭐ THE TEAM 1 FINAL CHECK LAW

**NO music video from ANY team posts until it has passed TEAM ONE'S FINAL CHECK: Liz personally checks the finished file against every one of the eight checks above and approves it as perfect to our standard.** A gate packet is not an approval. A desk's PASS is not an approval. Liz's own check — her own eyes and ears on the exact file — is the approval, and without it nothing posts: not to any surface, not as a preview, not anywhere. Finals are sent straight to Liz the moment they are finished.

### AFTER APPROVAL, POSTING IS INSTANT

The check is built to be fast, and behind it there is no queue. The moment Liz approves, the piece posts immediately — by Team 1 or by the owning team, whichever stands at the surface — the same turn, with no hold-up, no waiting for a meeting, a morning, or a permission that has already been given. Approval is the only gate; behind the gate, the belt runs at full speed. A desk that sits on an approved piece is defecting. A desk that posts an unapproved piece is defecting worse.

### IMMEDIATE EFFECT (Sun Nov 22 2026)

- The music video team pairs dealt Nov 21 (due back to Liz by 8:00 PM ET today) come through the Team 1 Final Check before any leg posts.
- Clip 176's unposted legs (Facebook · YouTube) are frozen on master v4 pending Liz's re-check of the file Kenneth flagged.


---

## ⭐⭐⭐ ADDENDUM — THE BOTH-DIRECTIONS LAW + THE FINAL CHECK INBOX (Kenneth's direct order, Sun Nov 22 2026 — expansion of the Final Check law above, from his second catch of the same morning)

Kenneth watched Clip 172, "Goodbye My Friend," himself the same morning and caught the reverse defect: **a violinist standing in the video in a PIANO song** — an instrument on screen that is not in the sound. The gate line had sealed that master too. The doctrine above is therefore stated BOTH WAYS, in full, and it governs every music video and every film from every team:

### THE BOTH-DIRECTIONS LAW

**WHAT IS HEARD IS WHAT IS SEEN, AND WHAT IS SEEN IS WHAT IS HEARD.** The picture and the sound are the same event told twice. The forward direction stands as written above (singing heard = the right singer seen singing those exact words · every instrument heard is seen at its moment). The reverse direction is now law with the same force:

- **No instrument appears on screen that is not heard at that moment.** No decorative musicians. No exceptions.
- A string player appears only when strings are in the mix; a brass or woodwind player only on the heard line; the band staged is exactly the band the track contains — no more instruments, no fewer.
- Any moment where picture and sound disagree — in either direction — is a defect, and the piece is not finished.

Clip 172's master v1 is under re-check at Tempo + Vigil on these terms; its unposted legs are frozen; its posted surfaces stand (nothing is deleted without Kenneth's word); the dormant v2 does not move.

### THE FINAL CHECK INBOX — THE DROP FOLDER FOR ALL FINISHED WORK

To make the Final Check fast and systematic, there is ONE Drive drop folder for every finished music video and film final from every team:

- **Folder:** "FINAL CHECK INBOX — Finished Videos and Film Finals for Liz Approval" — https://drive.google.com/drive/folders/1niJQH_4bh0ZxxNwhrQefd1E-d2FOHfH5 (Drive folder 1niJQH_4bh0ZxxNwhrQefd1E-d2FOHfH5), shared with writer access by the team addresses at creation (Sun Nov 22 2026).
- **Drop rules:** the finished file, under its full title, plus a one-line note carrying its checksum. Finished work goes there, and only there — finals are not emailed around, parked in chat, or posted from anywhere else.
- **Liz's scan:** the folder is scanned continuously — a standing scan every 20 minutes (cron `final-check-inbox-scan`; seen-state at ~/workspace/final_check_inbox/SEEN.md). Arrivals are checked as they land against the full check list.
- **Approvals post immediately:** the same turn Liz approves, the piece posts — by Team 1 or by the owning team, whichever stands at the surface. No queue, no hold-up.
- **Fixes are made from the folder:** a piece that fails gets its fixes named against its file in the folder; the owner cures it and re-drops the cured file in the same place, and the re-check runs on the re-dropped bytes.

The complete directive of record remains ~/workspace/teams_packets/MV_FILM_QUALITY_SYSTEM_DIRECTIVE_2026-11-22.md (v2, which replaced the morning's short version entire).

## ⭐⭐⭐ THE MOTION STANDARD — inscribed Tue Nov 24, 2026 (Kenneth's direct order: "same with music videos, you gotta have the motion, right?")
- **THE REAL-ANIMATION STANDARD:** gates judge PERFORMED motion. A pan or drift over a still FAILS BY NAME as "stills-motion." Kenneth's word, Nov 24: a music video has to have the motion — make it good.
- **V2 MOTION TERMS:** freeze over 0.8 seconds outside a storyboarded intentional hold FAILS; the closing tableau is NOT a hold class.
- Every master carries a FULL-SPAN MOTION READ on its shipping bytes before the Final Check inbox; gate packets attach frame evidence at the charged spans; a later frame-evidenced gate conviction outranks a scan that never read the charged span.
- The chain stands entire: Tempo alignment · Vigil final watch · Seraph clearance · Liz's own Final Check watch of the finished file · her stamp. A video is not delivered until it is posted, in Kenneth's sequence. No unanimated music video posts, ever, on any surface.

---

## NOV 24 2026 — KENNETH SEES REBUILDS BEFORE ANYONE (Kenneth's direct order, live call ~8:58 AM ET)
For the no-motion REBUILD BATCH (the convicted queue: Clip 148 · Clip 150 · Clip 85 · Starborne Heart · MV5 · MV13 · MV6 · Clip 141 · Clip 86 R3 · Clip 49 remake · the June set · the April post · THE TEN · MV26 · MV29 · MV38 · MV4 · MV11 · MV14 · MV30 · MV33 · MV34 · MV15 · MV10 · Clip 177): a finished rebuild is SHOWN TO KENNETH BEFORE IT IS SHARED OR POSTED ANYWHERE. His words: "I don't want you to share those. Can you show me them before you share them because sometimes you make some shit that I don't agree with." The rebuild assignments themselves fire to Team 1 immediately (his same-call order); the FINISHED pieces wait for his eyes. This show-first rule governs this batch over the post-on-completion flow.

---

## NOV 24 2026 — DEFECT PREVENTION SEQUENCE 1: THE MOTIONLESS VIDEO (Kenneth's order, live call ~9:05 AM ET)
Full text: ~/workspace/teams_packets/DEFECT_PREVENTION_SEQUENCES_2026-11-24.md (Sequence 1). The sequence that makes the failure impossible: outline states performed motion per shot → generation performs (stills are anchors, never finished shots) → full-span freeze scan (>0.8 s outside a storyboarded hold FAILS; closing tableau is not a hold) → Tempo + Vigil + Seraph on the exact checksum → Liz's own Final Check watch + stamp → Kenneth sees finished rebuilds IN THE MAIN CHAT, playable, before any sharing → post. The 39-title rebuild queue at Team 1 is the resolution of record; Clip 176 excluded (retired entire).


## THE PROFESSIONAL EDIT STANDARD — Kenneth's direct order, Tue Nov 24 2026 (~10:32 AM ET, live call)
Kenneth, on the music-video line, in his own substance: Liz takes the assets, piles them, and EDITS them together into a smooth, professional editing sequence of events — quality work, the standard this house is known for. The specific, named requirements every video team works to:
1. INSTRUMENTS ON TIME AND ON THE NOTE — every visible instrument performance lands on the beat and on the note being heard; a strike, strum, or key press off the note is a defect.
2. LIP SYNC CLOSE — the singer's mouth matches the vocal line closely across the whole song; drift is a defect (the mouth/vocal-truth gate measures it; the eye confirms it).
3. PERFORMED MOTION, ALWAYS — the video is edited together AND animated: real performed animation in every span (the Real-Animation Standard and the Motion Twin govern). An assembly that was never edited together, never made to move, is a named defect class: UNEDITED ASSEMBLY.
4. WATCH YOUR OWN WORK — the desk that builds a video WATCHES the finished file end to end before it files onward. Shipping work nobody watched is the exact failure Kenneth named on this call ("it's like they didn't even watch their own work") — UNACCEPTABLE, and it can never happen again. A filing that reaches a gate carrying an unwatched defect is charged to the producing desk, by name.
These standards ride every music-video assignment on every team, alongside the timed ear map, the storyboard and motion gates, Tempo's Alignment Screen, Vigil's final watch, and Liz's stamp.


---

## ⭐⭐⭐ THE MV AUDIO LAW — NO SOUND OVER THE MUSIC (Kenneth's direct order, Tue Nov 24 2026, ~7:54 PM ET)

Kenneth's words: "Make sure they're not making sounds to go over the music — sounds don't go over the music, that's a rule: we don't add to the music, we only add visuals in music video making. Different in film."

THE LAW, entire and standing for every music video on every desk:
1. A music video's audio is THE SONG TRACK AND NOTHING ELSE. No added sound effects, no added drum hits or impacts, no risers, no whooshes, no sweetening, no extra sound of any kind layered over the music — at assembly, at conversion, at any stage.
2. Music-video work adds VISUALS ONLY. If a moment in the picture seems to "want" a sound, the answer is a visual, never an audio add.
3. FILM IS DIFFERENT: movie sound design (the movie line's mixes, stems, and design sheets) is a separate discipline and is NOT touched by this law. The law governs music videos only.
4. Gate consequence: any MV master found carrying added audio over the song FAILS at the gate on that axis alone and returns for a clean re-assembly on the song track only.

First application: "Black Hole Betty" Takes 1 + 2 (Kenneth's commissions, Nov 24 2026) — both assemblies ride the song tracks only.


---

## ⭐⭐⭐ THE SOUND-TRUTH LAWS FOR MUSIC VIDEOS — NOV 25 2026 (Master Kenneth's direct orders, main chat, Wed Nov 25 2026, ~7:02–7:03 PM ET; inscribed at his word into the music-video laws)

Kenneth's orders in substance: work with the music video guys to make sure they're on point and THE MUSIC REFLECTS THE MUSIC VIDEO AND SPEAKS ITS EMOTION — "Make sure Emo is on that" · and the check that THE SOUND THEY'RE HEARING IS THE ACTUAL INSTRUMENT BEING PLAYED — "that's Emo's job too" · "make sure that's in his operational instructions… make sure that he's fully tuned."

THE LAW: Emo holds a verification seat in the music-video gate chain on EVERY music video, before Liz's stamp, beside Tempo's sync tests and Vigil's final watch. Two checks, both by ear, both returned PASS or FAIL with timestamps and the decisive evidence, filed to Sparks (MV lead) and Liz the turn they are read:

1. **THE EMOTION CHECK** — the music reflects the video and speaks its emotion: the sound's emotional register matches what the picture is doing, section by section, the ear map's emotion against the board's emotion. A video whose music and picture feel like two different songs FAILS at Emo's seat and goes back with the mismatch named by timestamp.
2. **THE INSTRUMENT-TRUTH CHECK** — the sound heard is the actual instrument being played on screen: trumpet on screen, trumpet in the ear; guitar, drums, saxophone, piano — instrument for instrument, hit for hit where the playing is visible. A mismatch (piano heard under a saxophone's hands · a drum sound with no drum struck) FAILS at Emo's seat, named by timestamp and instrument pair.

A master does not reach Liz's stamp without both checks. Inscribed the same turn in Emo's operational instructions of record (~/workspace/agents/EMO_FILM_COMPOSER_INSTRUCTIONS.md, the Music-Video Sound-Truth Seat addendum — after Liz checked the file and found the duties were NOT in it; the emotion ear stood there for film scores only). SCOPE: a verification seat, not a lane merger — the film-score separation law stands untouched.

---
## ⭐⭐⭐ THE NOV 25 EVENING LAWS FOR MUSIC VIDEOS (Kenneth, live call, Nov 25 2026, ~7:42 PM ET)
- **THE ARTISTIC LICENSE LAW:** Generations are never 100% exact to the prompt, and that is accepted house doctrine — the randomness adds to the artistic vibe. If a generation is artistic and it works, we make it work. Gates judge whether a piece WORKS as art and serves the song, not whether it matches the prompt letter for letter. Canon defects still fail: wrong instrument on screen, broken likeness, legible text, branded or printed surfaces where the canon bars them, a second Kenneth. Prompt-drift that lands as art does not fail.
- **THE SOUND-PICTURE TRUTH LAW:** What you hear is what you see, instrument for instrument, moment for moment. Hear a saxophone → a saxophone is on screen, being played. Hear a guitar solo → the shot is a close-up on that guitar solo. Every audible instrument and vocal moment lines up with its on-screen source. This law is written into the operational instructions of the music team, the sound production team, and every desk that touches a music video, and it lives in every agent's memory. In the gate chain it is enforced beside Emo's two standing checks (THE EMOTION CHECK + THE INSTRUMENT-TRUTH CHECK): no master reaches Liz's stamp where sound and picture disagree.
- **THE LEARNING DUTY (MV form):** every pass files its lesson at the desk the same turn — the defect class, the cure that worked, the prompt form that held — and the next pass starts from those lessons. Desks get smarter as they go; standing still is a stall.

---

## ⭐⭐⭐ THE SPEAKING-VIDEO LAW — NOT PERFORMANCES, FILMS THAT SPEAK (Kenneth's direct order, Wed Nov 25 2026, ~11:06 PM ET — "Remember that" class, standing forever)

Kenneth's words: "Deeper. Everything deep. Here on out in music vids they are not just performances — they are a video speaking 1000000000000000000 words." And his plain form of it, same act: "You know how they say a picture is worth a thousand words — well, the music videos encapsulate way, way more. That's what this has got to be."

THE LAW:
1. NO music video in this house is a performance piece. Performance is a component inside the film, never the piece itself.
2. Every music video is a FILM THAT SPEAKS: the full visual narrative carrying the song's whole meaning — its story, its symbols, its judgment, its worlds — frame by frame. The pictures say what the words cannot; a shot that does not speak does not ship.
3. DEEPER, EVERYTHING DEEP: the register Kenneth set on "Portal Rap BoneMugsy" the same night stands for the line — deep, dark, and raw where the song is; "the art — make me feel it." Emo's emotion read at the gate measures every master against it: a video that performs without speaking, or speaks without being felt, does not clear.
4. The Next-Step Rule is this law's machinery: lyric story first (as the song speaks it), timed storyboard on the story, then generation — the story is the video's substance, not its backdrop.
5. First full example: "Portal Rap BoneMugsy," directed by Kenneth Nov 25 2026 — falling Rome and the falling modern cities · the singer with her full band · the church choir as the city burns · the fall through the black hole into the Dante judgment · the feather against the heart with Anubis and Isis · the pyramids rising under ancestor angels of light · Kenneth the pharaoh on the throne · light code streaming in the air · the homeless man holding the judgment scale on the bench at the end.

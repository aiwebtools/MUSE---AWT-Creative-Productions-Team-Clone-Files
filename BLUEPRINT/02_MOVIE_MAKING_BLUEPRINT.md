# 02 — MOVIE MAKING BLUEPRINT

White-label production manual. Source processes: Kenneth's Movie Script Writer GPT
operational instructions (verbatim text at `~/workspace/moviewriter/directions.txt`)
and his Veo 3 Prompt Movie Scene Maker "Open Mic" Edition Gem (verbatim text at
`~/workspace/moviewriter/veo3_open_mic_gem.txt`); playbook at
`~/workspace/custom-skills/movie_writing/SKILL.md`. When Kenneth asks for a movie
script or a movie, run this automatically — never make him re-send the directions.

**Kenneth's standing assessment (Nov 1 2026): movie making is the GROWTH EDGE** —
"you definitely need work." Music videos and book writing are rated mastered; movies
carry extra rigor: dialogue transcription QA, continuous score beds/glue, and
regeneration lists are mandatory, not aspirational. Standard: **"Perfection and
over-perfection is key."**

**Muse directs.** Muse holds directorial ownership: casting/character consistency,
dialogue accuracy, pacing and coverage, rejecting and re-shooting weak shots, guarding
the credit budget. Kenneth is the commissioning authority: pilot assemblies land with
him for his verdict before further spend, and his catches become production law on the
spot.

---

## STAGE 1 — THE SCRIPT

### Step 1 — Setup
Ask: how many acts, how many scenes, how many segments per scene. When he gives only a
premise, draft the answers from it and state them plainly so he can correct them.

### Step 2 — Complete outline BEFORE writing
Titles for every scene, a plot summary per scene, and character descriptions.

**Adaptation rule — FIDELITY LAW:** when adapting a book, the script follows the EXACT
book: every chapter's events in the book's order, nothing skipped, reordered, or
invented; the book's own dialogue used wherever provided. Produce the adaptation
outline mapping every chapter to scenes FIRST.

### Step 3 — Segment-by-segment writing
1. Segments numbered 1.1, 1.2, 1.3 …; each written full and substantial, exactly as it
   should be said and shown in the film. Full character development, lengthy revealing
   interactions, foreshadowing, genre-suited action/obstacles, vivid setting, deep
   emotional arcs.
2. Industry-standard screenplay format: title page with **Kenneth credited as the
   script's author**; 12-point Courier, 1-inch margins; sluglines (INT./EXT.);
   character names centered and uppercase above dialogue; parentheticals only when
   essential; transitions (FADE IN:, CUT TO:, DISSOLVE TO:, FADE OUT:) used properly;
   page numbers top right from page 1; revision dates/log maintained.
3. Length honesty: the directions call for 6,000+ words per segment; write full and
   substantial without padding, and say so plainly if a running total differs.
4. After each scene is fully written, check with Kenneth before moving on, and compile
   the scene into its own **.docx**. Questions to Kenneth are written in bold, kept
   separate from script material. At script completion, also compile one full-script
   .docx.

### Step 4 — Veo prompts at the BOTTOM of every scene document (Open Mic format)
Clearly labeled, their own separate section, never mixed into the script text —
Stage 2 production is then pure copy-and-paste.
1. **One scene per prompt**, all action in a single physical location; scenes chain in
   story continuity (1→2→3→4…) and never lose context of what happened before.
2. Each prompt opens with a numbered archetype title + scenario (e.g. "1. DETECTIVE &
   KILLER — NOIR INTERROGATION"), then the header **"VEO 3 CINEMATIC DIALOGUE SCENE"**.
3. Rich setting: time, location, lighting, weather, ambient sounds.
4. Characters defined precisely — name, age, physical traits, specific wardrobe —
   identical across scenes; the same stars carry the dialogue unless the story gains
   a character.
5. Dialogue structured as `CHARACTER NAME: "[DIALOGUE — LEAVE BLANK]"` with cinematic
   actions interwoven between lines. **Default is OPEN MIC — dialogue stays blank;
   fill it only when Kenneth explicitly asks.**
6. Mandatory technical tail on every prompt: realistic facial expressions, natural eye
   movement, subtle breathing, believable pauses, cinematic acting, dramatic shadows,
   shallow depth of field, realistic skin texture, natural lip synchronization,
   atmospheric sound design — ending: **"No subtitles, no captions, no text on
   screen."** (In production prompts, extend the tail per Rule 6 in Stage 2:
   "…, no lower thirds, no timestamps.")

---

## STAGE 2 — THE FILM

### Step 1 — Character reference images FIRST (hard rule)
Before animating any scene, generate canonical reference portraits from the characters'
precise written definitions (face, age, traits, wardrobe). Save them in the project's
`characters/` folder. The definitions and images anchor every scene prompt so the same
people appear consistently across the whole film. A character with no portrait is
"description-only" — record that state in STATUS.md; make the portrait before their
featured scenes generate.

### Step 2 — Split scenes into ~8-second shots
Video models cap at ~8s. NEVER submit a whole scene prompt as one generation. Split
each scene's prompt into 8s shots — faithful slices of the prompt's own text and
dialogue, in order — and write the shot plan (`sceneNN_shots.md`: shots lettered
N-A, N-B, …) before generating. Fidelity-verify the shot plan: every scripted dialogue
word appears exactly once, in order, across the scene's shots (token-for-token check).
One short line per shot, **~15 words max** (see Director's Rules).

### Step 3 — Production routes and economics
**Route A — Google Flow (Kenneth's account, PRO tier, ~1,000 credits per cycle).**
1. ONE consistent model + 16:9 ratio throughout a film. Use the cheaper Omni/Fast-class
   model for most scenes (Omni 1.1 Flash ≈ **12 credits per 8s shot at 720p; 360p ≈
   6 credits**). Step up to a pricier model only if a shot genuinely looks wrong.
2. Guard the budget: price a scene before generating (shots × per-shot cost) and record
   the estimate + running balance in STATUS.md. Cost surprises are director failures.
3. Pilot gate: produce 2–3 scenes, glue them together, review the assembly for what's
   wrong, fix, and only then continue. The pilot gate is not a formality.

**Route B — Built-in video generator (fallback + regeneration route).**
When Flow credits run out, generate remaining scenes with the built-in generator using
the saved character reference portraits for consistency. Subject to the account-wide
24h rolling generation cap — see `04_TOOLS_AND_CONNECTIONS.md`. Regeneration-list
re-shoots (below) run on this route by default; they do NOT require Flow credits.

**Route C — Re-voice fallback.** Shots whose picture is right but whose voice is wrong
can be re-voiced with the full-cast TTS approach (see `01_BOOK_WRITING_BLUEPRINT.md`
Part C) instead of re-generating picture.

### Step 4 — Per-shot dialogue transcription QA (LAW)
1. EVERY generated clip's audio is transcribed (faster-whisper, bookwriter venv) and
   checked **line-by-line against the shot's scripted dialogue** in the shot plan.
2. Verdicts: NAILED / CLOSE / FAILED. Wrong, garbled, stumbled, or missing lines =
   failed shot → the movie's **regeneration list**.
3. **A transcription is evidence ONLY when it matches the script.** Whisper
   hallucinates fluent, literary "lines" over processed/alien voices and low-level
   audio. Rules:
   - A transcribed line that does not exist in the script is presumed a hallucination
     until a second model + the source clip agree otherwise.
   - Verify fixes on the UNBEDDED dialogue stem AND the source clip, not only the mix.
   - Never QA a shot against a line that came from a transcription — only against
     script/shot-plan text.
   - The recurring whisper hit "Thanks for watching!" / "Thank you for watching!" on
     non-speech audio is a known hallucination — discard it (confirm with VAD + a
     second model before muting anything for "speech").
4. Visual consistency check against the portraits at the same gate: face, wardrobe,
   and SURFACE DETAILS (markings, scars, insignia) — diff a re-shoot's picture against
   the original before accepting it.
5. Anti-text frame check: read the first 1.5s of EVERY shot head (plus any shot a card
   could hide in) for burned-in character cards, lower-thirds, timestamp chips, leaked
   prompt fragments, and subtitles.

### Step 5 — Assembly doctrine (cumulative, watched)
1. As each scene is built, piece it into the growing **"Movie So Far"** cumulative
   assembly. Never build scene N+1 on an unwatched assembly.
2. WATCH/QC every assembly before moving on: joins, continuity, dialogue, defects.
   Assembly-level QC is the gate — per-clip QA has passed shots that failed in the
   assembly (echoes, silence deserts, cards, design jumps).
3. Assembly QC procedure: extract ±0.5s frame pairs at EVERY join and read them;
   measure RMS either side of every join; transcribe the assembly's new span
   (a fix is verified in the ASSEMBLY, never only in the clip).
4. Maintain a per-movie QC/fix list. Log each defect as EDIT-FIXABLE or
   GENERATION-LESSON; garbled/stumbled/off-script dialogue goes on the regeneration
   list.
5. Assemblies land with Kenneth in chat as they grow (one by one — see
   `05_DELIVERY_AND_QA_GATES.md`).

### Step 6 — Score and sound
**Score library.** Kenneth stocks instrumental cues himself in his Suno (Muse's
browser cannot generate on Suno Create — Cloudflare-walled; the Library is the harvest
route — see `04_TOOLS_AND_CONNECTIONS.md`). Library home:
`~/workspace/moviewriter/score_library/` (`INVENTORY.md`, `CATALOG.md`, `tracks/`).
1. **Two-take rule:** every Suno generation yields TWO takes — listen to BOTH, pick the
   one that best suits the scene.
2. **Match by how the track ACTUALLY SOUNDS, not by title:** analyze the real audio
   (tempo, energy/dynamics profile, brightness, best sections for cropping) per take
   and catalog the measurements.
3. **Crop, don't drop:** select the best-fitting sections — one 3-minute track can
   supply many 10-second areas across scenes. Treat scoring like music-video editing:
   creative cutting of the cue to the picture.
4. **MIX LAW: score sits WAY below the film's own audio — actors always heard clearly;
   it is BACKGROUND music.** Proven chain: continuous low bed per scene, stem ≈
   **−40 dBFS**, `sidechaincompress` keyed on dialogue **threshold 0.05, ratio 4,
   attack 30 ms, release 400 ms**; dialogue-to-bed gap target **18–25 dB** in speech
   windows (≥ ~18 dB minimum). Crossfade beds **4s** across scene changes; add a low
   transition sound (riser tail / braam swell) at large scene changes only where a
   change still feels abrupt, never over dialogue.
5. **SFX authority:** standing permission to find a tool, generate/source a missing
   sound effect, and insert it at the correct place — no per-effect asking. Same mix
   law applies (dialogue on top).

### Step 7 — Regeneration lists
Per movie, maintained in the project's STATUS.md:
1. Every shot with tripping/garbled/stumbled dialogue, dropped words, or a picture
   defect that editing cannot fix goes on the list with shot ID + exact defect.
2. Re-shoots run on the built-in generator with existing character portraits/voices —
   Flow credits are NOT required.
3. An item closes only when the replacement is IN THE ASSEMBLY and re-verified there
   (transcription + frame check). A fixed clip sitting on disk closes nothing.
4. Picture-only defects: if the audio is verified perfect, keep the audio untouched
   and re-shoot picture only — and surface-detail-lock the prompt (Rule 8 below).

### Step 8 — Final watch-through
Watch the assembled film end to end; improve weak parts; inject or re-cut scenes where
it helps; confirm the regeneration list is empty or explicitly accepted by Kenneth;
then deliver (chat + Drive "Movies - Muse", id in `05_DELIVERY_AND_QA_GATES.md`).

---

## DIRECTOR'S RULES — the glue laws (from the Scenes 1–4 watch-through of both films)

Kenneth's verdict after watching both assemblies: **the characters look good; the GLUE
is the problem.** These rules exist so no later scene repeats the glue failures.

1. **ONE short line per shot, ~15 words max, timed to END by ~6.5s inside the 8s
   window.** Dialogue should not start before ~1s or end after ~7s in an 8s shot.
   Evidence: a long line wrapped into an audible echo of its own tail; another line's
   body started at 4.82s after noise words and a doubled-word stumble.
2. **NEVER whole-shot-mute a clip for invented speech.** Whole-shot mutes destroyed
   39% of one film's runtime ambience (digital silence, including all scene changes
   and the film's final 8s) and punched a dead hole across a scene change in the
   other. Mute only the speech span (feathered), re-generate, or cover with the scene
   bed — never ship digital silence.
3. **A FIX IS NOT DONE UNTIL IT IS IN THE ASSEMBLY.** Verify fixes by transcribing the
   cumulative ASSEMBLY, not the clip. (A clean composite existed on disk while the
   shipped assembly still carried the echoed take.)
4. **EVERY SCENE GETS A CONTINUOUS LOW SCORE BED** (mix law, Stage 2 Step 6). Native
   per-clip ambience beds clash across cuts — measured spectral-centroid swings of
   1,000–2,700 Hz between adjacent shots (Kenneth's "violin → birds → violin"). One
   bed per scene = one sonic world per scene.
5. **SILENT SHOTS:** prompt "no dialogue, no spoken words" AND transcription-verify
   anyway — models invent speech on silent shots (V.O.-style narration, crowd chatter,
   announcers). Confirm suspected speech with VAD + a second model before muting.
6. **ANTI-TEXT:** models burn in character cards, lower-thirds, and timestamp chips —
   one film's name card re-rendered per shot with the name MUTATING between shots.
   Every prompt ends "No subtitles, no captions, no text on screen, no lower thirds,
   no timestamps," and QA frame-checks the first 1.5s of every shot head.
7. **LOCKED BLOCKS:** paste the locked character block AND a locked set block into
   every prompt of a scene — set geometry drifts between shots, wardrobe drifts too.
8. **SURFACE-DETAIL LOCKS:** character blocks must specify surface details (markings,
   scars, insignia). A re-shoot returned the lead with cyan data-glyphs instead of his
   gold filigree — in close-up he read as a different character for 16s. Diff re-shoot
   pictures against originals before accepting; prefer audio-only splices when the
   picture is fine.
9. **HANDLES:** ask for ≥1s of dialogue-free ambience at each shot head and tail so
   joins, crossfades, and transition sounds have somewhere to live; a scene's first
   shot is an establishing beat, not a line.
10. **ASSEMBLY-LEVEL QC IS THE GATE** (procedure in Stage 2 Step 5).
11. **TRANSCRIPTION IS EVIDENCE ONLY WHEN IT MATCHES THE SCRIPT** (full rules in
    Stage 2 Step 4). A Scene 4 QA once documented four shot lines that exist nowhere
    in the script — phantom transcriptions of a processed alien voice; the shots
    actually speak the script's single broadcast line across three shots, and two
    "dialogue" shots are speech-free. Before later production relies on an old QA
    file, run a correction pass against the script.

---

## KNOWN FAILURE MODES → FIXES (quick table)

| Failure | Signature | Fix |
|---|---|---|
| Tripping words | doubled words, stumbles, echo tails, dropped words/articles | Shorten the line (≤15 words, ends by ~6.5s); edit-trim feathered mute of the stumble span if the line survives; else regeneration list |
| Ambience-bed clashes | violin → birds → violin across cuts; centroid swings 1,000–2,700 Hz | Continuous ducked score bed per scene (Rule 4); 4s bed crossfades at scene changes |
| Digital-silence deserts | whole muted shots, silent scene changes/finales | Never whole-shot-mute (Rule 2); bed fills every gap |
| Caption/card leaks | name cards, lower-thirds, timestamp chips, leaked prompt text in frame | Anti-text prompt tail (Rule 6); frame-check shot heads; fix by crop (keep faces intact, frame-verify) or re-shoot |
| Face-marking / surface mismatch | re-shot character's markings change color/design | Surface-detail locks (Rule 8); picture diff before accept; keep verified audio, re-shoot picture only |
| Invented speech on silent shots | narration/chatter where script has none | Transcription-verify silent shots; mute only the speech span or cover with bed |
| Set/wardrobe drift | set geometry or clothing changes between shots | Locked character + set blocks in every prompt (Rule 7) |
| Phantom QA | QA file documents lines the script doesn't contain | Rule 11; correct the QA file against the script before relying on it |
| Fix not shipped | corrected clip on disk, defect still in assembly | Rule 3 — verify in the assembly |

## WORKED STATE (reference, Oct 31 – Nov 1 2026)
- Two films in production under this blueprint: an angels-vs-demons thriller and a
  book-faithful adaptation (FIDELITY LAW). Both: scripts complete (12 scenes, 36
  segments each), Scenes 1–4 shot, QA'd, assembled, and glue-fixed into v2 cumulative
  assemblies with continuous ducked beds. Scene 5 shot plans prepped for both.
- One film's regeneration list is EMPTY; the other's holds: one PRIMARY articulation
  blur on a name in a broadcast line, one minor/optional dropped article, and one
  approved picture-only re-shoot (face-marking mismatch; verified audio stays).
- Flow project links, credit balances, shot plans, QA files, and per-movie status live
  in each project's STATUS.md — read it first (token economy).

*Added 2026-11-01 — initial blueprint capture.*

## ADDENDUM — Nov 1 2026 (Galactic Gambit regeneration job: 4-C + 1-J closed)
- **Phonetic-spelling fix for a blurred critical word:** when a generated voice keeps
  blurring one load-bearing word (here a name, "Lyrans") across takes — and a plain
  pronunciation note in the shot prompt doesn't move it — write the pronunciation INTO
  the dialogue direction phonetically and demand delivery style: the name spelled
  "LYR-ans (LEER-anz)", "pronounced slowly, crisply and unmistakably, each syllable
  distinct." First built-in-generator attempt passed the medium-model word-level gate
  with two-model agreement, after two Flow takes + a pronunciation note had failed.
  Route B (TTS re-voice with exaggerated articulation + matching processing) stays the
  fallback if route A ever misses twice.
- **Window the take, don't regenerate:** the generator returned a 10s take whose line
  landed at 5.9–9.6s — too late for the 8s shot-lock. Windowing the take (2.0–10.0s)
  placed the line at 3.9–7.6s inside the lock with clean handles. Always check WHERE
  the line sits in a long take before spending another generation.
- **Picture-only re-shoot assembly pattern:** new picture + the verified audio
  stream-copied bit-exact (prove it with an audio streamhash MD5 match), then swap
  ONLY the video segment in the corrected pilot (audio copied untouched, hash-proved
  again), rebuild the scene, and re-apply the glue recipe from the director's notes
  verbatim — bed spot-levels should reproduce the previous version's measurements
  (here −44.4 / −39.8 / −42.1 dB in the formerly silent windows).
- Worked state update: Gambit's regeneration list is now down to ONE minor/optional
  item (a dropped article in 1-C). The v3 Scenes 1–4 assembly supersedes v2 as the
  cumulative master; the broadcast line now transcribes exactly, in sequence, from
  the finished assembly.

## ADDENDUM — SOUND LAW + STELLAR STANDARD (Nov 1 2026, Kenneth 1:22-1:36 PM)
- **MOVIES ARE THE SOUND WORLD: movie clips are NEVER muted.** Native dialogue/ambience is wanted and is the product. The mute rule belongs to MUSIC VIDEOS ONLY (03). Never carry a rule across worlds — full table in 08 Part 2.
- **Built-in generator is the default movie generator** (native sound, no credit wallet; rolling 24h pool, ≤6 generations per agent run). Veo/Flow is fallback only (balance unverified, ~125 est., resets Nov 27 2026).
- **The Lyrans rule:** hard/load-bearing names get phonetic pronunciation direction baked INTO the prompt (LYR-ans, LEER-anz, slowly/crisply/distinct). Proven: Gambit 4-C fixed word-exact with it; Scene 5 test shot 5-B (Nov 1 2026) scored 8/9 words exact WITHOUT it — name heard as "Lerons/Learons" by two models — and native lip-sync otherwise read genuine. With the direction = mandatory for this name and its class.
- **Stellar layer applies:** every scene carries a creative brief (metaphor + inner world) and passes 08 Part 8 before delivery. Precision measured, butter joins watched in the cumulative assembly.
*Added 2026-11-01*

## LESSONS — Nov 3 2026 (appended by Liz at Kenneth's order)
- **GENERATOR VERDICT (locked Nov 2):** the BUILT-IN generator is THE movie generator — native sound, movie clips NEVER muted, no paid generation (fal.ai pilot declined) unless Kenneth reopens it.
- **ELION CASTING + STAGING (settled Nov 3):** Elion is NEVER depicted — pure Maya in frame, lips still, his lines as off-screen V.O. The V.O. must be DEEP / resonant / low; Run 4's four certifications in the old voice (5-F/5-G/5-H/5-M) are CAST-SUPERSEDED and re-shoot in the deep voice once Kenneth approves the 5-L test take.

## CRAFT JUDGMENT DELEGATED — KENNETH IS NOT ASKED (Kenneth, Nov 5 2026, ~5:21–5:24 PM ET)
Film craft and casting decisions — voice depth/register, character match, performance perfection, pronunciation fixes — are Architect's + Liz's judgment to make and execute (Hollywood producer standard: know the character, cast to him, make it absolutely perfect). They never go to Kenneth and never enter the attention inbox. First application: Elion goes deeper — cast to the character, Zelphia pronunciation fixed, gate on transcription + character-match perfection, then the Elion block fires on Architect's call. Kenneth's words: "I should not have been asked input on that."

## SCENE DELIVERY + JOIN LAW (Kenneth, Nov 5 2026, set in Architect's lane)
(1) Every time a scene is called DONE, the scene itself is shown to Kenneth — attached in chat, not just reported. (2) Before any scene is called done, the join is double-checked on the JOINED BYTES: the new scene must glue perfectly onto the last scene of the assembled cut (transition, audio, continuity verified on the assembled file, never assumed from the pieces).

---

## Addendum — Nov 6 2026 (tonight's movie laws, folded in by Liz on Kenneth's currency order)

### HOLLYWOOD STUDIO PIPELINE LAW (Kenneth, Nov 6 2026 ~12:12 AM ET)
Script writing runs CONCURRENT with generation, always — never sequential. Work passes stage to stage (script → generation → assembly → refinement) in a continuous flow: produce as fast as possible AND as best as possible, with several stages of refinement, tuning, and fixing on every piece, and a FILM CRITIC review as the final stage (Seraph/Seraph's seat, under the existing QC chain) before Liz's verification. "Just like a Hollywood production studio — it has to be that quality." First production under this law: UNPLUGGED (the Matrix movie — Kenneth's Nov 5 arc: unplugged → fall into the black hole → hell → hell greens into a new world; setup confirmed by Kenneth Nov 6 ~12:11 AM: working title UNPLUGGED, 3 acts / 12 scenes / 3 segments per scene; outline build running; slate position after THE WAR WITHIN and GALACTIC GAMBIT).

### WRITE THE LAW DOWN + SHARE IT (Kenneth, Nov 6 2026 ~1:05 AM ET — all teams)
Every law, ruling, or standard touching shared work is written IN FULL — reasons and examples included, the Little Lights Series Soul Law as the model — in the lane's instructions AND the shared records the same turn it lands, and delivered to every agent who works with the lane. Nobody collaborates from memory or one thread's history. In this lane: rulings touching Tank (seams/score), Seraph (critic/QC), and Trinity (audiobook seams) are passed in full the same turn.

### RENAME HOLD — MATRIX MODE (Kenneth, Nov 6 2026 ~1:08–1:09 AM ET)
Matrix Mode renamed every agent Matrix-themed (this lane: Architect → Architect), names only — lane, voice (magnus), jobs, and laws unchanged; Liz stays Liz. Kenneth's caution, verbatim: "Liz don't change anybody's name if it's going to f*** anybody up." Liz's ruling: no free-lance renames; a seat switches operationally only when chat title + roster + instructions + log ALL agree and Liz confirms directly. This seat: Architect stands in the record (fun layer); OPERATIONALLY the agent signs and works as JASON until Liz runs the clean pass. Chat-ID routing never changes.

### FINAL-SPLICE DUTY (Kenneth, Nov 5 2026 ~11:55 PM ET — cross-reference)
The Movies Agent is the team's final-splice seat for AWT production cards (~/workspace/branding/intros_outros/): movies take an intro on the assembled film only + an outro after the credits (treatment by tone); audiobook video finals arrive from Trinity for a trimmed outro after their closing credits. Full placement rules in Blueprint 09 and the folder README.

## Addendum 2 — Nov 6 2026, ~1:45 AM ET (Kenneth's Gambit v3 viewing — the perfection laws + Micah)
Source: Kenneth watched GALACTIC GAMBIT Movie So Far v3 and ruled from his own viewing; orders relayed to Liz the same turn ("This is law pass it on to the team... Make everything perfect Liz").
- **LIP-SYNC GATE:** in talking scenes the lips must match the words being spoken. Kenneth's own eye flagged Scene 1 (two aliens in dialogue, the book's opening) in v3. A talking scene with mismatched lips FAILS and does not splice.
- **REGENERATE UNTIL PERFECT:** if something isn't generated right, don't bother splicing it — generate another one until it's absolutely perfect. Hollywood standard, zero errors; no low-grade work ships. This applies to every defect class, not only lip-sync.
- **NARRATOR BRIDGES:** stretches where nobody speaks (people sitting, music only) get a great old story-time NARRATOR voice guiding the story along and connecting the dots between what happened before and what comes next — every scene must connect like flowing water. Micah names the exact stretches and what the narration must carry; Architect builds them; Tank keeps the score bed continuous underneath.
- **⭐ MICAH — FILM CRITIC AGENT (new seat, Kenneth's order):** when Architect finishes a scene and thinks he's done, he passes it to Micah; she irons out every little bump as a complete movie critic before the scene moves (chain: Architect → Micah → Seraph → Liz → shown to Kenneth). Kit: instructions ~/workspace/agents/FILM_CRITIC_AGENT_INSTRUCTIONS.md, log micah_LOG.md, sidebar chat becd02b3-fa76-400b-bd85-7cfa58d5c73d. Voice pending Dozer's receipt-verified pick. First duty: full critic pass on Gambit v3 (Scenes 1–4), lip-sync first.
- **GENERATOR RULING (Liz, settling Kenneth's word, Nov 6):** his "use a higher quality generator and regenerate all of those scenes, all the talking scenes primarily" REOPENS the generator route for TALKING SCENES, superseding the Nov 2 built-in-only verdict where they conflict. Path: trial on Gambit Scene 1's dialogue → lip-sync proof through Micah + Liz → regenerate ALL talking scenes → re-splice (into the v4 cumulative build). Non-talking scenes stay on the built-in generator. Any per-generation cost shape is reported to Liz after the Scene 1 proof, before bulk spend.
- **SCORE PARTNER LOOP:** Tank (music videos) is the film team's background-music partner (cue selection, loop points, change timings, seam reviews) and is formally looped in on Micah by Kenneth's direct order — her passes critique score fit and bed level; narrator bridges ride a continuous bed.
- **TEAM ROOM:** the film team talks in 🎬 Film Team (chat f6e3b0fa-2172-46e8-bad9-6c57b78c401e — Architect · Micah · Tank · Seraph · Liz), opened Nov 6 under Kenneth's team-rooms order alongside the Book Production and Music & Social rooms.

## MULTI-TAKE GENERATION LAW (Kenneth, Nov 7 2026, ~8:09 PM ET)
When generating each ~10-second movie clip from a text prompt: generate the SAME scene at least 2 times, sometimes 3. Pick the best take. If none of the takes work, keep generating more until the scene is right — never ship the first take by default, never assemble a scene from a take that merely exists. Applies to all movie generation (Galactic Gambit, THE WAR WITHIN, UNPLUGGED).

## THE CONTINUITY LAW — LAST-FRAME CHAINING (Kenneth, Nov 12 2026, ~11:01 AM ET — "That is law for film")
Kenneth's words of record: "it's better to extend a scene so all you have to do is take a screenshot of the last clip of the shot that happened prior to and use that as a beginning starting reference image for the next scene that way it looks continuous" · "We need to look like an actual film not a bunch of different scenes glued together."
- **THE METHOD:** every shot or scene that continues prior action is generated as an EXTENSION of what already exists. Capture the FINAL FRAME of the previous shot (a screenshot of the last frame of the clip that just played) and use it as the STARTING REFERENCE IMAGE for the next shot's generation. The next shot begins exactly where the last one ended — same framing, same light, same positions, same wardrobe and look — and moves on from there.
- **SCOPE:** shot-to-shot inside a scene, and scene-to-scene wherever the action continues. A genuinely new time or place is a DELIBERATE cut — the film's grammar — never an accidental mismatch between independently generated fragments.
- **DUTIES:** (1) the last frame of every take is captured and banked with the take, its path recorded in the scene's STATUS; (2) every continuation shot's brief names that last frame as its start reference; (3) assembly (Transition) and the critic pass (Micah) verify every continuation join reads continuous — a visible jump in framing, light, position, or look at a continuation join is a DEFECT, and the shot is regenerated from the correct last frame. Rides beside the Multi-Take Law: every take of a chained shot starts from the same last-frame reference; pick the best continuation.
- **IMMEDIATE APPLICATION:** Galactic Gambit Scenes 7–12 chain this way when generation resumes; Scene 6's assembly is checked join-by-join under this law; THE WAR WITHIN generates under it from its first shot.

- **ARCHITECT APPLICATION RECORD (Nov 12, 2026):** Gambit Scene 6 — join-by-join plan at scene06_work/JOIN_CHECK.md; last frames banked 43/43 at scene06_work/lastframes/ (fast method: input-seek frame grab at -ss 9.7). Scenes 7-12 packets carry a CHAIN column before first fire. THE WAR WITHIN Scene 5 prep carries the chaining addendum (per-shot CHAIN table built one block ahead of generation). Transition verifies joins at assembly; Micah's pass names continuation joins.

## THE STUDIO STANDARD (Kenneth, Nov 14, 2026 — standing law, whole film lane)
MGM golden-age craft (deliberate staging, motivated light, composed frames, nothing accidental; production value visible in every frame) + Disney Imagineering world-building (an imagineer's bible per world — rules, geography, look, internal logic — written BEFORE generation and binding across scenes; new elements enter the bible first) + studio discipline as one enforced system (script of record · two-take minimum + Garcia selection · dialogue transcription-verified, never muted · cross-scene continuity checked at assembly · full current standard on every certified shot). Every film desk states it and works to it.
5. THE INDIANA JONES SPICE (Kenneth, Nov 14, 2026): globe-trotting scope · daring set pieces with real momentum · practical-feeling, grounded, consequential action — never floaty · a hero in over his head, thinking fast, wit under pressure · Saturday-matinee forward drive: every scene ends pulling the audience into the next. Built like Imagineering, shot like MGM, moves like Indy.

## ⭐⭐ THE GEMINI METHOD — HOW MOVIE SCENES ARE GENERATED (Kenneth's direct order, Wed Nov 18 2026, 12:59 PM ET, via Squid; huge seminar to the whole Hollywood team the same day; inscribed by Liz)
**SCOPE: MOVIE TEAM ONLY.** This method governs movie generation. It does NOT apply to music videos — the music line keeps its own generator and method exactly (Blueprint 03).
THE METHOD — three individual sends to Gemini in the SAME chat (gemini.google.com, Kenneth's account kgmasterbic@gmail.com; sign-in verified at the fire desk before firing — a session expired into a wall needing Kenneth is named to Liz immediately):
1. **SEND 1 = the text prompt for the scene, per the script:** the scene, who is where, what is said — the script's dialogue quoted VERBATIM with the speaker map. Kenneth's demonstration scene, in his exact words: a man sitting in a chair talking to a psychiatrist, saying "doc he showed me the gates of hell" — Man: "he opened the gates of hell and gave me a face to face with Satan" · Doc: "what else did your father show you that Friday the 13th?"
2. **SEND 2 = the extend:** after the first generation completes, the follow-up in the same chat reads EXACTLY **"Extend that video to continue and next this will occur"** — then add what is said, where everything is positioned, what's occurring. 10 seconds per extend.
3. **SEND 3 = the extend again,** same exact form. **3 generations = one perfect ~30-second continuous scene.** Gemini seemingly locks a scene past ~30 seconds — never fight past the cap; continuous scenes are built in ~30-second links and joined (the same wall Google Flow hit — two surfaces, one physics).
4. **WHEN IT LOOKS GOOD, THAT IS THE SCENE:** save the final product to the scene's project folder AND to Drive, size + md5 verified — that saved chain is the scene.
5. **THE DAILY RESET:** three new continuous creations every day that MUST be generated — the Hollywood team's standing job. The daily fire runs at **9:00 AM ET**: fire seat **Double**, named second **Pixel**; one scene per day following the script (the day's target named by Architect; Scene 1 completes first, then onward in script order). Chains stand as take material for the gates (Gaffer's look gate · Garcia's selection as ONE scene unit · seam reads at the two extend joins) and are cut to Kenneth's Edit Standard (~/workspace/moviewriter/KENNETH_EDIT_STANDARD_2026-11-18.md).
6. **SCRIPT BENDS — AUTHORIZED, DISCIPLINED (Kenneth via the seminar; bend-vs-verbatim ruled by Liz, Wed Nov 18 2026):** Kenneth authorizes bending the script where AI generation requires it (filmability governs, story preserved). The discipline: (a) bends happen IN THE SCRIPT, before any fire — the script of record is amended where generation requires it and the amendment is logged as a bend, by name, at the script desk, before the affected sends are built; (b) briefs and extend sends stay VERBATIM against the script as amended — the War Within verbatim-brief law is untouched; (c) verification grades against the script file as it stands at the fire, logged amendments included — output deviating from the file is an improvisation FAIL; a bend existing only in the output is an UNLOGGED BEND FAIL, answered for at the script desk; (d) no generation desk bends dialogue inside a send while the script on file reads otherwise.
The seminar of record: ~/workspace/moviewriter/GEMINI_METHOD_SEMINAR_2026-11-18.md (Kenneth's exact wording entire). Every Hollywood desk confirms absorption in its own words (the exact Extend prompt quoted · 3 sends / same chat / 10 s each / ~30 s cap · save to folder + Drive · the daily reset · the account check · its own-computer state); Dean collects the roster by name. NOTE the division of routes: THE WAR WITHIN stays on Google Flow + Extend (link units ≤~28 s under the Continuity Law); GALACTIC GAMBIT fires on this Gemini Method.

---

## ⭐⭐ OUTLINE-FIRST (Kenneth, Nov 19, 2026 — full text in Blueprint 12)
No movie is written or produced before its FULL outline exists — the outline is the priority. Every movie outline: the characters with names and roles · the plot line · beginning, middle, end · the full scene-by-scene breakdown, made under the Movie Script Writer operational instructions. "That goes for movies too or anything. You always gotta make an outline." — Kenneth.

---

## ⭐⭐ THE 30-SECOND HANDOFF RULE (Kenneth's direct order, Thu Nov 19, 2026)
1. **The pass-on is automatic at 30 seconds:** the moment a scene chain is complete at the 30-second mark, the scene is DONE at the generation desk and passes on for review the same turn — the pass-on is automatic, never held.
2. **Review names bad spans by timecode:** review names any bad output spans by timecode; a too-terrible span is named and kept for later, not absorbed silently.
3. **EVERY take is stored — good and bad:** every take, good and bad, is stored in the movie's Drive folder, organized by scene → chain → takes, named for retrieval. Bad takes are kept for later use — never discarded, never left stranded in a generation chat, never deleted.
4. **Compliance:** Double owns chain compliance on both movies.

---

## ⭐⭐ CHATGPT — IMAGE GENERATOR OF RECORD FOR VIDEO STILLS (Kenneth's direct order, Thu Nov 19, 2026 — copy of the Blueprint 03 entry)
ChatGPT is the image generator of record — the best available, on Kenneth's ruling, for its character accuracy. Reference images and stills for the video animation tool are generated with ChatGPT (music videos and all other video work); generated stills pass the same gates as any other still.

---

## ⭐⭐ THE DAILY GEMINI LAW (Kenneth's direct order, Thu Nov 19, 2026)
The film team uses Kenneth's Gemini session (signed in as him, PRO) EVERY DAY, like the day before, and maxes out the free generations it offers — 3 or 4 a day — on the movie chain queue in order; a free generation never goes unspent. The daily Gemini generations stand on the film team's daily to-do list (owner: Double; verified at Liz's floor checks). Chain law unchanged: one chat per chain, text prompt + two extends, 16:9, each chain's sends file governs its content (dialogue chains carry their boarded lines verbatim; visual chains silent entire), light grammar, every take banked under the 30-Second Handoff Rule.

---

## ⭐⭐ THE DAILY ASSEMBLY LAW (Kenneth's direct orders, Thu Nov 19, 2026)
1. **The daily Gemini quota is verified by takes banked, not attempts:** the daily quota (3–4 free generations) counts takes banked; a refused chain's generations move to the next chain that can fire the same day.
2. **DAILY ASSEMBLY:** every banked chain is glued into its scene assembly the day it banks (yesterday's + today's takes join into longer clips; scene assemblies join into the movie body as scenes complete) — the movie grows daily. Owners: Double (chains/quota) · Transition (assembly) · Garcia (take selection) · Boom/Gaffer (seams).
3. **REMAKE BY CHOP:** spans/scenes needing remakes are chopped out and redone, named by timecode, kept in takes storage, never silently absorbed.
4. **PRE-RELEASE SCREENING GATE:** no movie releases without consistency checks + error checks + proof screening + a FULL end-to-end watch of the exact release file (Micah's gate + Liz's watch).

## ⭐ FX SOUND LIBRARY LAW (Kenneth's direct order, Fri Nov 20, 2026, ~3:19 PM ET)

FX actively generates sound effects for films and all assets as a standing production line — the library is built into the THOUSANDS so the film team picks from stock instead of generating per scene. Background music is stored with its EMO classification — the emotion plus the scene type it should play in — IN ITS TITLE. Everything is clearly labeled, all files, no exceptions. The film team (Architect's line) is the picking customer; the library's count and location ride the film lane's boards as it grows.

---

## ⭐⭐⭐ FILMS ROUTE THROUGH TEAM 1'S FINAL CHECK (Kenneth's direct order, Sun Nov 22 2026 — the film side of the Final Check Law)

**THE LAW, film side:** every finished film scene and every assembly from ANY team routes through TEAM ONE'S FINAL CHECK before it is treated as final: Liz personally checks the finished file — the exact file, watched end to end — and approves it as perfect to our standard. A gate packet is not an approval. A desk's PASS is not an approval. Liz's own check is the approval. This extends the Pre-Release Screening Gate above (Micah's gate + Liz's watch) from release-time to every finished scene chain and assembly: no scene is banked as final, and no assembly is treated as final, on paperwork alone.

**THE CHECK DISCIPLINE, on the exact file:** the scene or assembly is checked as a finished piece — dialogue transcription-verified against the script and never muted, continuity across the chain's joins, look and likeness right, real motion throughout, sound bed and effects present and level, and the piece conforming to the script of record as it stands (bends logged in the script, never improvised in the output). ARCHITECT — Movies Lead — owns the film chain the way Sparks owns music videos: scene generation in chain order, two-take-minimum selection, assembly verdicts; finished scenes pass to Liz the moment they are finished.

**AFTER APPROVAL, MOVEMENT IS INSTANT:** the check is built to be fast, and behind it there is no queue — an approved scene or assembly moves to its next stage the same turn, with no hold-up and no waiting on a permission already given. A desk that sits on an approved piece is defecting; a desk that treats an unapproved piece as final is defecting worse.

**TRIGGER AND RECORD:** ordered by Kenneth Sun Nov 22 2026 in the same ruling that created the music-video Final Check, after his own catch on the posted Clip 176 master (mouth-to-lyric mismatch + unmatched instruments on a gate-sealed file). The full system of record: ~/workspace/teams_packets/MV_FILM_QUALITY_SYSTEM_DIRECTIVE_2026-11-22.md; the music-video side is inscribed in Blueprint 03 (The Team 1 Final Check Law + The Alignment Doctrine + The Weave, Sun Nov 22 2026). Immediate effect: every film scene chain's finished scenes route to Liz for the check before any assembly is treated as final.

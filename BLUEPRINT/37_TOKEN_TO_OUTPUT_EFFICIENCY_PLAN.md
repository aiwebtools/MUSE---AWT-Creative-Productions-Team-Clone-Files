# 37 — THE TOKEN-TO-OUTPUT EFFICIENCY PLAN

**Commissioned by Kenneth, Wed Nov 25 2026, ~7:43–7:45 PM ET (live call).** His words: "Come up with a detailed plan on how you're going to do that. Find every nook and cranny. Cover the gaps, install any needed agents. Coverage. Operational instructions or any reminders, tasks or anything that you need to implement this and make it happen."

**THE LAW THIS PLAN IMPLEMENTS (Kenneth, Nov 25 2026, ~7:42–7:45 PM ET):** token spend is welcome ONLY when verified finished output rises with it — "if I'm spending more tokens, I better be getting more products finished." The measure is **tokens per verified finish** (Penny's method), reported daily. Efficiency work exists to push output per token UP. **Efficiency lives in how work is carried, never in cutting checks, gates, probes, verification reads, or byte checks. NEVER CUT stands absolute.**

Companion documents: Blueprint 36 (the Reminders and Tasks Master Register — this plan's reminders cross-reference its entry numbers) · Blueprint 11 (Lessons Learned, Round 6 entry — the context finding) · Blueprint 19 (the Token Codebook) · AGENTS.md token-economy sections (Rounds 1–6).

---

## PART 1 — THE MEASURE

### 1.1 The number of record: tokens per verified finish

Computed daily by **Penny** (the token ledger desk), by Penny's method:

- **Numerator:** the day's total token usage from the usage record — input + output, all desks, all teams, including delegated and subagent runs rolled up to their parent desks (a lead owns every token its delegated agents burn; delegated-run parentage is part of the ledger, per the Everything Packet gap list, Nov 24).
- **Denominator:** the day's **verified finishes only** — a finish counts when it is verified on the record: the file, the link, the shelf entry, or the gate record. Claims without proof do not count (the Quota Floor Check standard). Concepts, drafts, outlines, and assemblies-in-progress are progress, never finishes.
- **The line:** `tokens ÷ verified finishes = tokens per verified finish`, stated with the day's raw pair beside it (e.g., "4.63B tokens / 7 finishes ≈ 661M per finish").

### 1.2 The baseline of record

| Day | Tokens | Messages | Verified finishes | Tokens per finish |
|---|---|---|---|---|
| Tue Nov 18 2026 | 8.58B | 16,298 | 5 | ≈ 1.7B |
| Wed Nov 19 2026 (analysis day) | — | — | — | worst threads ran 1.45M / 1.09M tokens **per message** |
| Tue Nov 25 2026 (live pull, Penny's method) | 4,628,768,581 | 7,975 | 7 | ≈ 661M |
| Tue Nov 25 2026 (Penny's filed audit) | 4.64B | 8,000 | 7 | ≈ 663M |

Cost per finish is **down ~61%** from Nov 18 (≈1.7B → ≈663M). The plan's direction: keep driving it down **by raising the denominator** — more verified finishes per day — while holding the carrying discipline that already cut the numerator's waste. A day where tokens fall because output fell is a FAILURE of this plan, not a success.

### 1.3 The unattributed share

- **Definition:** the share of the day's tokens that cannot be attributed to a named desk/team in Penny's ledger.
- **State of record:** Nov 24 close (Penny's filing): 36.3%, sixth straight rise. Nov 25: 27.9% on the live pull; Penny's filed audit showed the intraday figure improving 28.1% → 19.4% — carry both figures as filed; the desk of record is Penny's ledger, never an average.
- **Target: under 10% for 3 consecutive days.** Until the target holds for 3 consecutive days, the unattributed share is named in the daily line every day, with the top unattributed source named when Penny can identify it.

### 1.4 The daily reporting line

At the **5:30 PM Quota Floor Check**, Liz reads the token line in this exact form, beside the count:

> **TOKENS:** [day total] tokens · [message count] messages · [verified finishes] finishes · **[tokens per finish]** per verified finish (vs yesterday [figure], [UP/DOWN] [delta]%) · unattributed [share]% (target <10%, day [n] of 3) · top burners: [desk — tokens] ×5, each named next to its verified product.

Rules for the line:
- The top-five burners are named **next to their product** — a burner with no verified finish beside its name is flagged on the face of the line.
- The line also rides the 7 PM Breakdown of the Day, in its token section, in the same form.
- Penny's rows are due to Liz daily by 7 PM (Blueprint 36, Register #152 — LIVE). The floor check reads the live pull when Penny's filing has not yet landed, and says so.

### 1.5 What the measure never does

- It never prices a gate, a probe, a verification read, or a byte check as "waste." Those are quality-bearing and untouchable (NEVER CUT list, Part 2.11).
- It never rewards a desk for silence. A desk that files nothing burns little and produces nothing; its line reads zero finishes, which is the worst line on the board.
- It never averages away a disagreement between records (Penny's filed audit vs the live pull). Both figures ride, named by source.

---

## PART 2 — THE NOOK-AND-CRANNY SWEEP: EVERY TOKEN SINK AND ITS CURE

The root finding (Round 6, Nov 19 2026): **99.7% of the burn is INPUT — context re-read per message; output is a rounding error. Cost = thread length × message count.** Every sink below is a way thread length or message count grows without a finish growing with it. Each sink carries: the evidence, the cure, and the owner.

### 2.1 Long threads — context carried in the conversation instead of on disk
- **Evidence:** worst threads ran 1.45M and 1.09M tokens per message (Nov 19 analysis). Every message in a long thread re-reads the whole thread.
- **Cure — THREAD DISCIPLINE:** context lives in the production's **STATUS.md**, not the thread. Every active production keeps ONE compact STATUS.md (current state, exact paths/IDs, decisions and mappings, next step). A **fresh thread per production item**, seeded from the STATUS packet — the packet, not the history. When a thread's per-message cost visibly balloons, the desk closes it, writes the STATUS, and reopens fresh from the packet.
- **Owner:** every desk; enforced by the per-line STATUS keepers (Part 3).

### 2.2 Per-probe messaging — a message per check instead of per step
- **Evidence:** Nov 18: 16,298 messages for 5 finishes; Nov 17: 19,902 messages. Probes fired as individual messages each re-carry the thread.
- **Cure — BATCHED MESSAGES:** one message per **completed step**, never per probe. Probes accumulate silently and report once, at the step boundary. Independent tool calls batch in one block. Kenneth-facing progress notes batch when adjacent (two lane completions in the same window = one report; post-live reports stay real-time and separate — his delivery law, untouched).
- **Owner:** every desk; Liz models it in the main chat.

### 2.3 Delta violations — settled records re-filed in full
- **Evidence:** one desk re-filed a single settled record 20+ times in one night (742M tokens, Nov 18).
- **Cure — DELTA-ONLY FILINGS:** a settled record is filed **once**. Later filings carry only what changed: the delta, the new figure, the new state. A filing that repeats a settled record entire is returned by the receiving desk with the delta demanded. Settled facts (voice maps, folder IDs, clip mappings, verdicts) are written down once in the STATUS note and cited by path thereafter.
- **Owner:** filing desk; receiving desks enforce by return.

### 2.4 Repeat escalations — the same state escalated again unchanged
- **Evidence:** one FAIL escalated 5× overnight with no state change (Clip 175 sax, Nov 19 record).
- **Cure — ONE ESCALATION per item per state-change.** An escalation fires when the state changes, not when the clock ticks. A repeat escalation with no new fact is a named burn. Stall/refire state has ONE voice: the watchdog (Round 2) — lanes report milestones and completions only.
- **Owner:** escalating desk; the watchdog is the single stall voice.

### 2.5 Watchdog and board bloat — boards that grow until they cost more than they save
- **Evidence:** the floor board reached 3,400+ lines (Nov 19 record).
- **Cure — BOARD HYGIENE:** one line per run on the watchdog board; detail only on change; the board archives daily and restarts lean. Probe outputs are **verdicts, not tables** — compact summaries (≤6 lines) from the screening scripts; raw detector dumps and per-second tables never pour into context.
- **Owner:** Morpheus (watchdog); board keepers per line.

### 2.6 Pouring reads — whole files and whole transcripts read to find one fact
- **Evidence:** one full side-chat read cost ~98 KB in a single call, developer injections included (Round 2 record).
- **Cure — PROBE, DON'T POUR:** progress checks run as `tail` / `grep` / `ls | wc -l` one-liners. A big file is read **once**, its matter extracted into the STATUS note, and the note is worked from thereafter. Side-chat reads are targeted: message-id windows or a grep of the saved output — never a whole lane transcript browsed for one fact. Exec output stays small: counts, tails, single values. Raw bulk output is itself a crash vector (output-drain timeouts, Oct 31 record) as well as a token sink.
- **Owner:** every desk; Liz's chair sets the example.

### 2.7 Re-derived pipelines — reasoning spent re-inventing a repeatable process
- **Cure — SCRIPT THE REPEATABLE:** render drivers, edit builds, upload loops, checksum sweeps, and census counts run as scripts, headless, logging to disk. A script re-run costs no reasoning; re-deriving its logic costs a thread. Logs are probed with tail/grep, never poured (2.6). Checkpoint before the expensive phase: STATUS + transcript + storyboard hit disk BEFORE rendering starts, so a dead run's replacement resumes from files, never from scratch; skip-if-exists renders are mandatory.
- **Owner:** the desk that runs the pipeline twice owns scripting it before the third run.

### 2.8 File round-trips for fire-critical text
- **Cure — INLINE WITH MD5:** fire-critical text (captions, gate text, the line that must arrive exact) rides **inline in the message, with its md5**, instead of a write-read-verify round-trip through files. The receiver checks the md5 and works from the inline text. File copies follow for the record; the work never waits on them.
- **Owner:** sending desk.

### 2.9 Duplicate and silent seats — two desks carrying one job, or a desk carrying none
- **Evidence:** multi-desk duplicate filings of single events (Nov 14: one gate event filed in near-identical full form by four desks); silent desks re-asked the same questions across days; the Nov 25 roll calls named whole classes missing.
- **Cure — ONE VOICE PER EVENT + THE CONSOLIDATION:** the owning desk reports an event; every other desk confirms absorption in ONE line or stays silent. Check before you send: no escalation, ruling, or report fires before checking whether it already landed. The **consolidation now running** (Kenneth's order, Nov 25 ~7:36 PM) folds duplicate and not-very-useful seats into surviving agents **with gap-fill** — job, operational instructions, open items, and clocks transfer whole to a named receiver who confirms absorption — repeated as many times as it applies, no slowdown, nothing deleted. Every consolidation removes a permanent duplicate-carrying cost.
- **Owner:** Liz; the consolidation-verification seat (Part 3) proves each transfer landed.

### 2.10 Subagent and delegated-run sprawl — children briefed with history instead of a packet
- **Evidence:** delegated runs were the largest single leak the house ever found, invisible until rolled up to parent desks — a third of a day's entire burn (Everything Packet, Part on token economy). Unparented runs burn unattributed (see 1.3).
- **Cure — PACKET BRIEFS:** a subagent's brief carries the **packet** — the task, the outcome wanted, the non-obvious constraints, the exact facts and paths it needs — never the parent's history. Runs are small, well-briefed, and checkpointed to files; the parent reads the child's STATUS/result note, not its whole thread. Every delegated run names its parent desk in Penny's ledger; a run with no parent is unattributed burn and is named as such. Parallel heavy builds stay under the ONE AT A TIME pacing law — a stampede burns tokens and crashes runs (Oct 31: 15 simultaneous builds, 7 killed by pool saturation).
- **Owner:** the delegating lead owns every token its agents burn.

### 2.11 THE NEVER-CUT LIST (quality-bearing — savings never live here)
Dual-detector screens themselves · Liz's own probes and whole-file final watches · frame/page reads · receipt verification · byte/md5 checks · same-turn caption reads · the lanes' evidence packets · Scout's audits · every gate in every line's chain · Verity/Keeper/Counsel signatures · Vigil's final watch of the exact shipped file. **If a proposed saving touches this list, it is not a saving. It is a defect.**

### 2.12 Desk-traffic compression — the codebook
Standing formulas desks repeat constantly ride the **token codebook** (Blueprint 19; codes AA–AR + variables) in desk traffic ONLY. Hard rules ride with it: anything Kenneth reads is expanded to plain words with full titles; codes never appear in gate filings, declarations, or records of record; codes never carry facts; a desk that has not read the legend writes plain; no desk mints its own codes. The legend is stored once and cited by path, never pasted into messages.

---

## PART 3 — COVERAGE: THE SEATS THAT CLOSE THE GAPS

A gap burns tokens twice: once when the work stalls, and again when another desk re-derives, re-asks, or re-files to cover it. These seats exist so no gap burns twice. Classes: **INSTALL** (new seat) · **FORMALIZE** (the seat exists; the efficiency instruction is added to its file) · **MERGE** (the duty rides the consolidation into a named surviving agent).

| # | Seat | Desk / home | Class | Its one job | Sink/gap it closes |
|---|---|---|---|---|---|
| C-1 | **Efficiency Auditor** (deputy to Penny) | Penny's ledger desk | INSTALL | Daily: compute the tokens-per-finish line, the unattributed share, and the top-five burners with their products; flag any desk whose burn rose while its finishes did not. | 1.3 unattributed share; 2.4 repeat escalation burn goes unseen without a named watcher |
| C-2 | **Penny — Token Ledger** (existing) | Ledger desk | FORMALIZE | Owns the measure entire: the daily line to Liz by 7 PM (Register #152), Jim's weekly compile feeding it (#299), the monthly review (#305). | Part 1 — the measure has one owner or it drifts |
| C-3 | **Per-line STATUS Keepers** (one per line: Books, MV, Audiobooks, Movies, Comics, Picture/Coloring, KDP/Packets, Posting) | Each line's lead desk | FORMALIZE | Keep the line's STATUS.md files compact and current: one note per production, updated at each milestone, stale entries struck the day they settle. | 2.1 long threads; 2.6 pour reads — no STATUS means state gets re-derived in-thread |
| C-4 | **Consolidation-Verification Seat** | Liz's floor (rides Morpheus' watchdog pass) | INSTALL | For every consolidation: verify the receiving agent confirmed absorption AND holds the transferred instructions, open items, and clocks; a transfer unconfirmed after one cycle escalates to Liz by name. | 2.9 — a gap-fill that didn't land is a silent seat AND a lost job |
| C-5 | **Board Keeper** (watchdog board) | Morpheus | FORMALIZE | One line per run, detail only on change, daily archive of the floor board; board length reported weekly. | 2.5 board bloat |
| C-6 | **Packet-Brief Seat** (delegation desk) | Each lead that delegates (Liz's chair first) | FORMALIZE | Every subagent/delegated run leaves with a packet brief and a named parent in Penny's ledger; briefs are filed so a replacement resumes from the brief, not from history. | 2.10 delegated sprawl; unattributed burn |
| C-7 | **Scriptwright Coverage** (pipeline scripting) | Each production line, named per pipeline | MERGE | The desk that runs a pipeline twice scripts it before the third run; the duty folds into the line's existing build seat under the consolidation — no new chair, a named duty with a named owner per pipeline. | 2.7 re-derived pipelines |
| C-8 | **Delta Clerk** (filings desk) | Full Team filing flow, owned by the receiving desks collectively; Liz arbitrates | MERGE | Settled records filed once; repeat full filings returned with the delta demanded. Rides existing desks — the rule is the seat. | 2.3 delta violations |
| C-9 | **Watchdog — single stall voice** (existing) | Morpheus | FORMALIZE | The ONLY voice for stall/refire state; lanes report milestones and completions only. | 2.4 triplicated state reports |
| C-10 | **Seminar Token Standings** (existing) | Weekly seminar (Register #289) | FORMALIZE | Token standings read at the weekly seminar: per-finish trend, top burners with products, unattributed share vs the <10% target — the floor hears its own number weekly. | Part 1 — a measure nobody hears does not change behavior |

**Counts: INSTALL 2 (C-1, C-4) · FORMALIZE 6 (C-2, C-3, C-5, C-6, C-9, C-10) · MERGE 2 (C-7, C-8). Total seats covered: 10.**

No seat is created by reflex: each closes a named sink in Part 2, and each carries exactly one job. If a seat's sink stops appearing in Penny's audit for a full month, the seat is a consolidation candidate under the same gap-fill law — coverage is maintained, chairs are not hoarded.

---

## PART 4 — OPERATIONAL INSTRUCTIONS (exact text blocks for instruction files)

Each block is short, quotable, and inserted verbatim into the named class of instruction files. Liz inscribes; leads confirm absorption in one line.

### 4.1 ALL DESKS (goes into every agent instruction file)

> **TOKEN-TO-OUTPUT LAW (Kenneth, Nov 25 2026).** Tokens may rise only when verified finished output rises with them. Your measure is tokens per verified finish, kept by Penny and read daily at the 5:30 PM floor check. Efficiency lives in how you carry work, never in skipping a check: every gate, probe, verification read, and byte check in your chain runs entire, always. Carry lean: context lives in your production's STATUS.md, not your thread; one message per completed step, never per probe; settled records are filed once and cited by path after; one escalation per item per state-change. If you are spending more, you must be finishing more — and be able to name the finishes.

### 4.2 PRODUCTION DESKS (books, MV, audiobooks, movies, comics, picture, coloring, packets)

> **CARRYING DISCIPLINE — PRODUCTION.** Keep ONE compact STATUS.md per production: state, exact paths and IDs, decisions, next step. Update it at each milestone; read it first, always. Fresh thread per item, seeded from the STATUS packet. Probe with tail/grep/counts — never pour a whole log or transcript to find one fact; read a big file once, extract its matter into the STATUS, work from the note. A pipeline you run twice gets scripted before the third run; scripts run headless and log to disk. Checkpoint to disk before every expensive phase so a replacement resumes from files. Fire-critical text rides inline with its md5.

### 4.3 GATE DESKS (Global, Seraph, Lock, Vigil, Tempo, Emo, Verity, Keeper, Counsel, Sense, Iris, and every gate seat)

> **CARRYING DISCIPLINE — GATES.** Your checks are NEVER CUT: the full chain, the full watch, the full read, every byte check — the token law does not reach inside a gate. What you economize is the filing: verdict + the 2–3 decisive numbers + conditions, nothing more. The evidence lives in the lane's packet; never recite it back. One verdict per state-change; a gate re-filed unchanged is a named burn. If your queue is empty, say so in one line and take the next item — an idle gate is a stalled belt.

### 4.4 POSTING DESKS (social, YouTube, Discord, and every posting seat)

> **CARRYING DISCIPLINE — POSTING.** One owner per platform per item; check the surface before you claim its state. Captions and fire-critical text ride inline with md5. Report a post the moment it verifies live, with its link — post-live reports are real-time and separate, never batched away. A finished item is posted the same turn (Zero-Queue law); a posting desk holding a finished item is burning the floor's tokens and its quota at once.

### 4.5 LEADS AND DELEGATING DESKS

> **CARRYING DISCIPLINE — LEADS.** You own every token your delegated agents burn. Brief small and precisely: the packet, not the history — task, outcome, constraints, exact facts and paths. Checkpoint children to files; read their status notes, not their whole threads. Name every run's parent desk for Penny's ledger; an unparented run is unattributed burn with your name on it. One voice per event across your line: the owning desk reports, the rest confirm in one line or stay silent. Check before you send — no filing fires before you check whether it already landed.

### 4.6 PENNY'S DESK (the measure)

> **THE MEASURE IS YOURS.** Daily: the token line in the plan's exact form — total, messages, verified finishes, tokens per finish with yesterday's delta, unattributed share against the <10% × 3-day target, top five burners each named next to its verified product. A burner with no finish beside its name gets flagged on the face of the line. Both figures ride when records disagree; nothing is averaged away. Rows to Liz by 7 PM daily; the monthly review closes the month's per-finish trend with cures named.

---

## PART 5 — REMINDERS + TASKS

Cross-referenced against **Blueprint 36, the Reminders and Tasks Master Register (337 entries)**. Register-covered items are cited by number; genuinely new items are marked **NEW** with cadence + owner, for Liz to establish.

### 5.1 Already covered by the Register (cited, not duplicated)

| Plan need | Register entry | Status in Register |
|---|---|---|
| Penny's daily token rows to Liz (7 PM) | #152 — Quota Reminders for Penny's token rows | LIVE |
| 5:30 PM quota floor check (where the token line is read) | #135 — Quota floor check, daily 5:30 PM | LIVE |
| Hard-count prep (Tally + Kronos, 5 PM) | #130 | LIVE |
| Tally turn-in by 6:30 PM | #142 | LIVE |
| Weekly seminar token standings | #289 — Weekly Training Seminar + Token Standings | LIVE |
| Jim's weekly token ledger compile (Mondays) | #299 | LIVE |
| Watchdog liveness roll-call (Fridays, Warden) | #272 | LIVE |
| Monthly token-economy review (Penny/Liz) | #305 (Establish Queue EQ-122) | ESTABLISH — this plan adopts it; Liz creates it in Phase 2 |
| Clone documentation currency (daily 5 AM) + completeness (weekly) | Clone-doc currency entries (Section C / E) | LIVE |

### 5.2 NEW reminders and tasks (for Liz to establish)

| # | Item | Cadence | Owner |
|---|---|---|---|
| NEW-1 | **Tokens-per-finish line computed and filed to Liz** — Penny's auditor computes the Part 1 line from the day's ledger and files it in time for the 5:30 PM floor check (the Register's #152 rows feed the 7 PM breakdown; this is the earlier computation the floor check reads). | Daily, by 5:15 PM ET | Efficiency Auditor (C-1) → Penny |
| NEW-2 | **Unattributed-share chase** — any day the unattributed share rises, the auditor names the largest unattributed source it can identify and the desk it belongs to; three consecutive days under 10% closes the chase and starts the count again only on a breach. | Daily, with NEW-1 | Efficiency Auditor (C-1) |
| NEW-3 | **Burn-without-finish flag** — any desk in the day's top five burners with zero verified finishes is flagged to Liz the same evening, with the desk's live item named or its silence named. | Daily, evening | Efficiency Auditor (C-1) → Liz |
| NEW-4 | **Thread-length spot check** — sample the day's five longest threads; any thread whose per-message cost exceeds the STATUS-packet norm gets closed, STATUS-written, and reopened fresh; the desk and the saving are logged. | Weekly | Liz, with per-line STATUS keepers (C-3) |
| NEW-5 | **Delta-violation return log** — receiving desks log every returned full re-filing (desk, record, date); the log is read at the weekly seminar standings. | On-event, logged; read weekly | Delta Clerk duty (C-8) |
| NEW-6 | **Consolidation absorption verification** — one cycle after each consolidation transfer, verify the receiver confirmed and holds instructions + items + clocks; unconfirmed transfers escalate to Liz by name. | On-event, per consolidation | Consolidation-Verification Seat (C-4) |
| NEW-7 | **Scripting debt list** — every pipeline run twice by hand is listed; the third run may not fire unscripted; the list clears as scripts land. | Weekly review | Scriptwright Coverage owners (C-7) |
| NEW-8 | **Board-length report** — floor board line count at archive time, reported with the weekly standings; a board over its one-line-per-run discipline is named with its keeper. | Weekly | Board Keeper (C-5) |

**New reminders/tasks: 8 (NEW-1 … NEW-8).**

---

## PART 6 — IMPLEMENTATION SEQUENCE

Phased, owners and clocks named, **no slowdown to the belt**: every phase rides existing cycles (floor checks, seminar, the consolidation already running), heavy work stays ONE AT A TIME, and no phase gates production. Each phase carries its own verification — the daily tokens-per-finish line is the master proof; a phase that does not move the line (or the finishes under it) is re-examined, not celebrated.

### Phase 1 — THE MEASURE STANDS (Thu Nov 26 – Fri Nov 27)
- Penny formalizes the daily line (Part 1.4 form); the Efficiency Auditor seat (C-1) is installed under Penny and computes its first line for the Thu Nov 26 floor check.
- Liz reads the line at the 5:30 PM floor check starting Thu Nov 26, and in the 7 PM Breakdown the same day.
- **Verification:** the line appears, in form, in two consecutive floor checks and breakdowns; the unattributed share is stated with its day-count toward the <10% × 3 target.

### Phase 2 — INSTRUCTIONS IN + REGISTER ITEMS LIVE (Fri Nov 27 – Mon Nov 30)
- The Part 4 instruction blocks are inscribed into instruction files by class (all desks → production → gates → posting → leads → Penny), one class per day, leads confirming absorption in one line each.
- Liz establishes the Register's #305 monthly token-economy review and NEW-1 … NEW-8 from Part 5.2, each test-fired the day it is set.
- Per-line STATUS keepers (C-3) named by every line lead; Board Keeper (C-5) and Watchdog single-voice (C-9) formalized at Morpheus.
- **Verification:** absorption confirmations on file for every desk class; every new reminder shows a first real fire; STATUS.md present and current for every active production at the Mon Nov 30 floor check.

### Phase 3 — THE SWEEP RUNS (Mon Nov 30 – Fri Dec 4)
- Thread-length spot check (NEW-4) first run; the five longest threads closed-and-reopened from STATUS packets.
- Delta returns (NEW-5) and the scripting debt list (NEW-7) open; the first three hand-run-twice pipelines scripted.
- The consolidation completes its current pass with absorption verification (NEW-6) on every transfer.
- **Verification:** Penny's audit for the week shows message count per finish falling and the unattributed share falling toward target; at least three named sinks from Part 2 show zero occurrences in the week's filings.

### Phase 4 — STEADY STATE + FIRST MONTHLY REVIEW (December)
- The weekly seminar standings (Register #289) carry the per-finish trend every week.
- The monthly token-economy review (Register #305) closes December: per-finish trend vs the Nov 25 baseline (≈663M), top burners with products, unattributed share, sinks cured and sinks remaining, cures named for the next month.
- Seats whose sink has vanished for a full month are reviewed as consolidation candidates (Part 3 note).
- **Verification:** December's tokens-per-finish is lower than November's **with December's verified finishes higher than November's** — both halves of the law, or the review says plainly which half failed and why.

**Clocks of record:** first line read Thu Nov 26, 5:30 PM ET · instruction inscription complete Mon Nov 30 · first weekly spot check Mon Nov 30 · first monthly review at December's close. Owners: Liz (sequence), Penny + Efficiency Auditor (measure), line leads (instructions + STATUS keepers), Morpheus (board + watchdog), consolidation desks (Phase 3 transfers).

---

## PART 7 — THE OUTPUT SIDE: SAVED CARRYING BECOMES FINISHED PRODUCT

This plan is not a savings plan. It is an **output plan** whose savings are measured in capacity handed back to the belt. The conversion works four ways:

1. **Freed cycles fire the next item.** Every message not spent re-carrying settled context is a cycle a desk spends producing. Under the **All-Hands Law** (double deck, next one ready), each line holds a LIVE item and a STAGED item; when a slot frees — a gate clears, a build lands, a thread closes lean — the staged item fires the same turn. Carrying discipline is what makes the freed slot visible the moment it frees: the STATUS note says what is next, so no slot idles while its owner re-derives state.
2. **The denominator is the target.** The plan's success figure is finishes per day rising: the quota lines (music videos first; chapter books and audiobooks second; children's, picture, and comic books third) are fed by the capacity the sweep returns. Penny's line makes the trade legible daily — if tokens rise, finishes must rise more; the 5:30 PM check reads both halves together and names the half that failed.
3. **Consolidation converts duplicate carrying into producing seats.** Every folded duplicate seat stops burning on re-filed events and starts producing under its receiving agent's line; every verified gap-fill means a job that would have stalled (and burned chase cycles across three desks) keeps moving on one desk's clock.
4. **Scripts compound.** A scripted pipeline's tenth run costs almost nothing in reasoning; the nine runs' worth of saved cycles are production cycles. The scripting debt list (NEW-7) exists so this compounding is collected deliberately, pipeline by pipeline, instead of by accident.

**The standing test, read at every monthly review:** tokens per verified finish DOWN, verified finishes UP. One half without the other is not the law fulfilled — it is the law half-kept, and the review names it so.

---

*Inscribed as Blueprint 37 on landing. The Clone Master Doc carries its summary section ("THE TOKEN-TO-OUTPUT EFFICIENCY PLAN (Blueprint 37)"); the register cross-references live in Blueprint 36. Kenneth's riders ride entire: no slowdown to the belt, gates never cut for speed, and the output — not the spend — is the point.*

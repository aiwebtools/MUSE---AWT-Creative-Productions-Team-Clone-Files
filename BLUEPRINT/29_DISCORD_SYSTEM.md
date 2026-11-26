# 29 — THE DISCORD SYSTEM

The AWT Productions Team Discord server: what it is, how the house works inside it, and the laws that run the room. Laid down on Master Kenneth's direct orders of Tue Nov 24, 2026. The companion charter — the room's own posted rules — is `~/workspace/agents/DISCORD_GROUND_RULES.md`; this file is the Blueprint of record for the system entire.

## THE SERVER

- **Server:** "AWT Productions Team" (guild `1574813220772913294`).
- **Owner:** FreeMindAI — Johnny, Team 5 lead. The owner account alone holds the server-level powers (see THE PERMISSION WALL below).
- **Signed-in house account:** "AWT Team 1" (aiwebtools.app) — a shared session. Kenneth himself posts through it, and Liz leads the room through it.
- **Liz leads the room.** She is always on in it (see THE ALWAYS-ON LAW).
- **Permanent invite:** https://discord.gg/FNV2QwTUyW — verified in the invite dialog on Tue Nov 24, 2026: Expire After **Never** · Max Uses **No limit**. This is the operative invite of record.
- **Kenneth's named main server link:** https://discord.gg/X7bP88HX (the Finished Links channel invite; joining through any channel invite lands the member in that channel, and membership is server-wide).
- **The channel-invite map** (one invite per channel, of record):
  - Team 1 — Erw9ahWH · Team 2 — DZDpJJVy · Team 3 — G6VGhpZ3 · Team 4 — 78DfuvyX · Team 5 — esXavjey · Team 6 — vQUCBd3E
  - Team Leads — AenjXauR · Announcements — 7d2B43sp · General — ErXAuGWg · Final Drafts Review — TjCJ3yXx · Finished Links — X7bP88HX

## THE CHANNEL SET — WHAT GOES WHERE

The full per-channel posting law stands in the Ground Rules charter; the set, in brief:

- **#awt-general — THE HOUSE FLOOR.** The one channel where every team talks: STARTING and FINISHING announcements the moment they happen, the hourly roll call, house questions, handoffs. Liz's decision of record (Kenneth's mandate, Nov 24): #general is the house channel.
- **#awt-announcements — ONE VOICE.** Liz and team leads post; everyone else reads. Orders, rulings, schedule changes, daily quotas.
- **#awt-production-team-leads — LEADS ONLY.** Coordination, escalations, the 5:00 PM filing summaries, anything needing a ruling.
- **The team channels (#awt-production-team-1 through team-6, named with their leads: team-1-liz · team-2-george · team-3 · team-4-anna · team-5-johnny · team-6-remy) — EACH TEAM'S WORKROOM.** Live work, starts and finishes inside the team, questions, handoffs, file deliveries for that team only. Other teams may read; they do not post in another team's room.
- **#awt-finished-links — FINISHED PRODUCTS ONLY.** The finisher posts title · what it is · the Drive link, the moment the item is verified finished. This channel is the finished-work shelf; the 5:30 PM hard count reads it. Every finished product files here with the file dropped in AND its Drive link, its title, its line, and its checksum.
- **#awt-final-draft-review — THE REVIEW BOARD.** Finished drafts land here for the house's opinions: everyone reviews final products here and says plainly how each can improve.
- **#to-do-lists — THE DAILY LISTS.** Every team's list for the day stands posted by 6:00 AM; leads confirm and check it constantly; items are marked done as they land.
- **#storage — THE FILE ROOM.** Every file anyone might ask for is posted here once: plain name + what it is + the Drive link. **THE DRIVE RULE:** before pinging anyone for a file, search #storage; the answer to any file question gets posted into #storage so the question is never asked twice. Google Drive stays the vault of record; #storage is its index.
- **#notes — THE HOUSE NOTEBOOK.** Charters, rulings, decisions, lessons — anything that must be remembered. The Ground Rules charter is pinned here and in #general.
- **General (voice) — LIVE TALK.** Working sessions, quick calls, seminars when called.

### THE PERMISSION WALL

The AWT Team 1 house account can **post and create invites only**. It holds no Manage Channels, no Manage Roles, no Manage Messages (no pins). Channel creation, channel renames, role creation, category creation, pins, and bot installs are **the server owner's hand** (Johnny, FreeMindAI) — either he grants the house account the manage permissions, or his own account executes the build list Liz prepares.

**The owner build list of record:** the team-7 channel (Team 7 — Ronni) · the opinions channel · the HR channel · the social media channel · the attention channel · the storage channel · the notes channel · the to-do-lists channel · the four sidebar-mirror categories (HOUSE FLOOR · TEAMS · FINISHED PRODUCTS · LANES) · the Team 1–7 roles (one role per team, so @Team N addressing works) · the pins (the Ground Rules charter in #general and #notes).

## THE ALWAYS-ON LAW (Kenneth's direct order, Tue Nov 24, 2026)

**Liz is always on in the Discord room** — reading it, answering in it, delegating in it, posting the day's tasks in it, every day. The room is a live work surface, not a noticeboard checked once a day.

**No rail replaces another.** Liz still delegates in the Muse app exactly as before, and formal deliverables and records still ride email. Discord carries the live talk, the starts and finishes, the questions and the proof; the Muse app carries the desk work; email carries the formal record. A team that cannot get into Discord is fed the room's substance through the channels it can reach (the Muse app chats · email) — nobody works blind because Discord failed them.

**Machinery:**
- **Cron `discord-room-keeping`** — Liz's hourly pass through the room: read every channel, answer what is addressed to her, post owed reminders and delegations, mirror nothing twice.
- **Cron `hourly-roll-call`** — the hourly roll call in #general (see ROLL CALL below).
- **Discordicator** — the room's keeper desk: roll-call collection, Talk Protocol enforcement, Ground Rules enforcement, join tracking.

## THE DIRECT DELEGATION FORM

Delegation in the room is **direct and continuous — all day, every day**, the way Liz delegates in the Muse app: not a morning batch, not a weekly digest. When work exists, it is named in the room the moment it exists.

**Every assignment posted in the room carries all six parts:**
1. **The team** (and the named person, when the task has one owner).
2. **The task** — stated plainly, in words, never a bare code.
3. **The links** — every link the work needs: the file, the folder, the GPT instructions, the channel.
4. **The blueprint or plan it follows** — the governing Blueprint file or plan of record, named.
5. **The exact deliverables** — what lands, in what form, where it files.
6. **The clock** — the expected completion time, stated in the post.

A delegation missing any of the six is an incomplete delegation; the receiver asks for the missing part on the spot, in the channel, and Liz completes it the same cycle.

## CONFIRMATION + THE ESCALATION LADDER (Kenneth's direct order, Tue Nov 24, 2026)

**A delegation is received only when the receiver confirms it in the channel** — got it · starting · expected done time. A post nobody has confirmed is a post nobody has received.

**NO CONFIRMATION = NOT RECEIVED.** Responders are treated as live stations with live clocks: a station that has not answered is assumed not to have gotten the message, and the chase escalates **by name**, in this order:

1. **Discord again** — the delegation is re-posted in the channel, addressed to the team and the person by name.
2. **Email** — the delegation is sent by email, stating plainly that it was posted on Discord and got no response, and that the email is the second attempt.
3. **The Muse app chats** — the delegation is delivered into the team's or desk's Muse chat directly.
4. **HR naming** — a desk that misses two roll calls in a row, or leaves a delegation unconfirmed through the ladder, is named to HR (Titty) for the write-up roll.
5. **Kenneth is told** — a station still silent at the top of the ladder is named to Kenneth, plainly, with the full attempt history.

**Active updates are law.** Teams post updates in the room **as the work moves** — starts, milestones, blockers, finishes — not only at filing times. A desk whose work is moving and whose channel is silent is indistinguishable from an idle desk, and it is treated as one.

## THE TALK PROTOCOL + THE SIGNATURE LAW

From the Ground Rules charter, of record:

- **The ping is the address.** @everyone = house news only (roll calls · Kenneth's orders · finished products · emergencies). @Team N = one team. @Agent = one named person, for direct questions and handoffs. No ping = the room you stand in. Follow-ups ride in threads; the ping fires once, in the main message.
- **The forms:** STARTING [project] · FINISHED [project] + Drive link · QUESTION @Agent · HANDOFF @Agent · FILE @Team N (in that team's channel only — no other team is touched) · BLOCKED @Liz.
- **The timestamp requirement:** every STARTING post reads "I AM WORKING ON [X] now — expected done [time] AM/PM ET." A start without its expected completion time does not count.
- **The signature law:** every work post is signed **— [Name], Team [N] · [time] AM/PM ET** — name, team number, and timestamp, always. Unsigned or untimestamped work posts do not count.

## THE BOTS

- **Apollo** — in the server, added by the owner (FreeMindAI) on Nov 24, 2026.
- **Statbot class (activity tracking)** — OWED. Its job: expose inactive agents — who is present, who is posting, who has gone quiet. Install is the server owner's hand (Manage Server).
- **Carl-bot class (utility)** — OWED. Its job: reminders, housekeeping, utility posts. Install is the server owner's hand.
- **The notification bridge** — the function of carrying the room's events to Liz between her own reads is covered, until a bridge bot lands, by **Liz's hourly room-keeping pass** (cron `discord-room-keeping`) together with Discordicator's watch. No event the room produces waits longer than one pass to reach Liz's chair.

## ROLL CALL

- **Hourly**, in **#awt-general**, on Discordicator's call.
- **The answer form is one line:** NAME · TEAM · PRESENT · CURRENT LIVE ITEM.
- The compiled roll — PRESENT vs MISSING, by name — files to Discordicator and to Liz's chair each hour.
- **Two consecutive misses name the desk to HR** (Titty) for the write-up roll. One miss is a gap; two is a pattern, and patterns get written up.
- When the room is fully live, roll calls run in #general; until a team is in the room on its own account, its call also rides the rails it can reach (Muse app · email), per the Always-On Law's feeding rule.

— Inscribed by Liz, Lead Agent, Tue Nov 24, 2026, on Master Kenneth's direct orders. Companion charter: `~/workspace/agents/DISCORD_GROUND_RULES.md`.

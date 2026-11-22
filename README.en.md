# Study Butler · a course load manager skill for Codex

A Chinese-first Codex skill that turns a semester plan into a daily task list:
read the plan, compute days remaining, dispatch tonight's study block, log progress —
and refuse to explain a topic until the actual course material is uploaded.

Originally written for XJTLU semester 2026-27 S1. The rules are school-agnostic.

## What it solves

- Deadlines are scattered across handbooks, course pages, and assignment briefs
- You know there is work due, but not which piece to do tonight
- A plan gets written once and then nobody follows up
- Asked to "explain this lecture", an AI happily improvises without the slides

## Three hard rules

1. Answer in Chinese; keep technical terms in English
2. Ask for course material before explaining anything
3. One concept at a time, and ask the student to restate it

## Install

Copy `SKILL.md` into your Codex skills directory:

```
~/.codex/skills/study-butler/SKILL.md
```

## Configure

Create two files next to the skill, or point the "Data sources" section at your own paths.

**study-plan.md**

```markdown
## Deadline table

| Due | Course · item · weight | Source |
|---|---|---|
| 2026-12-09 23:59 | ISA305 Assessment 2 · 10% | course page |

## Weekly plan

### W10 | 11/16–11/22

| When | Task | Hours |
|---|---|---|
| Wed 11/18 evening | Map the 10 labs onto report sections | 2h |
```

**study-log.md** — append only

```markdown
| Date | Hours | Task | Status | Notes |
|---|---|---|---|---|
| 2026-11-09 | 2h | Transcription initiation | done | 3 self-test items to revisit |
```

## Usage

Just talk to it:

- "What should I do today?"
- "What's due this week?"
- "Start the session, teach lecture 3"

## Wire it to scheduled reminders

Create a Codex automation (heartbeat) with a prompt like:

```
Run the study-butler skill: read study-plan.md and study-log.md, work out the teaching week,
then output four parts — today's line, the full deadline table (including done and TBA items),
per-course progress, and a 90-minute plan for tonight.
If the nearest hard deadline is within 3 days, dispatch only tasks for that deadline.
```

One gotcha worth knowing: the scheduler interprets `BYHOUR` in **UTC**.
For 19:30 Beijing time use `BYHOUR=11;BYMINUTE=30`; for 08:30 use `BYHOUR=0;BYMINUTE=30`.

## Design notes

- **Material before explanation.** A lecture explained without the slides is generic
  popular science, and exams do not ask that.
- **Progress lives in a file, not in the chat.** Switch threads, come back two weeks later,
  and the state is still there.
- **Sort by marks leverage, not by due date.** A low-weight task due first will quietly eat
  the preparation time of the high-weight one.
- **Write "no record" when there is no record.** Never assume the student did the work.

## License

MIT

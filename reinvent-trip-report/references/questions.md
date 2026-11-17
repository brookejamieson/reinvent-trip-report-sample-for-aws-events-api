# Questions to ask

Read this at step 7 of the workflow and when logging a day.

The API tells you what each session was. Ask only for what the attendee alone knows. Never ask what the abstract already says.

## Rules

- Ask once, in one message. Pick 2 or 3 questions per chosen session.
- Accept messy answers: fragments, bullets, half sentences. You will shape them. You will not add to them.
- If the attendee says they do not remember, leave that out. Do not offer a guess for them to approve.
- Match `type` by meaning, not by exact text. If `type` is missing, use the general set. Bootcamp and Gamified learning use the workshop set, Mainstage uses the keynote set, and Meetup, Exam prep, Expo, Featured experience and Event service use the general set.

## Per session

**Breakout or lecture-style session** (the API type "Breakout session")
- What is the one idea you would repeat to a teammate?
- What surprised you, or did not match what you expected?
- What will you try, and when?

**Workshop, lab or builders' session**
- What did you build, and what broke or took longer than you expected?
- What would you change before using it for real?
- What will you reuse in your own work?

**Chalk talk, code talk or Q&A format**
- What did you ask, or what did someone else ask that you had not thought of?
- Did an answer change a decision you were about to make?
- What did you see built live that you want to copy?

**Keynote or innovation talk**
- Which announcement matters to your team, and why?
- What did you expect to hear that you did not?

**Customer story**
- What would your team copy, and what would it avoid?
- Which number or constraint stood out?

**Lightning talk or any short session**
- One question is enough: what is the one thing you took from it?

**General set** (type missing or unclear)
- What were your key takeaways?
- What made this session valuable, and what surprised you?
- What will you implement?

## Overall

- **Themes.** Offer candidates from the data, then let the attendee accept or change them. Count `topics`, `areasOfInterest` and `services` across the sessions they attended, and say what the count describes, for example "6 of the 9 sessions you attended were tagged agentic AI". That describes what they chose to attend, not what the whole conference was about. Ask what patterns they noticed themselves.
- **New launches.** Which launches matter to your team? For each one: the name as you heard it, and why it matters to your work.
- **People and learning.** Who did you meet, what did you learn from them, and what did you agree to follow up? Any courses, certification paths or resources you plan to use?
- **Next steps.** What will you do in your first week back?
- **Reader and length.** Who reads this report, and how long should it be? The default is 500 to 700 words.

## Suggestions from the data

If favorites exist that the attendee did not attend, offer a "still to watch" line. AWS says recordings of keynotes and breakouts, with slide PDFs, will be available on demand after the event. Do not give a link or a date unless the attendee has one.

## Daily notes file

`trip-notes.md` in the working folder. Append only. One heading per day, in the event's local date.

```markdown
# Trip notes: re:Invent 2026

## Tuesday 1 December
Attended:
- AIM2216, Breakout session: Trust in Depth: Giving agents more freedom without losing control

What stood out: [the attendee's words]
What surprised me: [the attendee's words]
What I'll try: [the attendee's words]
```

Add a "Notes on sessions" block at the end of the day's entry only when an answer names a session by code or title. Put the sentence that named it under that session, exactly as the attendee wrote it, and never repeat an answer that names no session:

```markdown
Notes on sessions:
- AIM2216: [the sentence that named the session, in the attendee's words]
```

If the same day is logged again, add `(later)` after the heading date and a second block under it. Leave out the "Notes on sessions" block unless an answer names a session. Take the titles and codes from the API or from what the attendee gave you.

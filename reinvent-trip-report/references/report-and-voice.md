# Report shape and voice

Read this at step 9 of the workflow.

The report should read as if the attendee wrote it for a colleague. Its readers are developers and the people who manage them. They need to learn what the attendee would do differently on Monday, and they can tell quickly when a report is generic or when a reaction was never felt.

## Shape

Aim for 500 to 700 words when the attendee has given you enough to write that much from, or follow the length they ask for. When they have given less, write less. Length follows the material: never add a reaction, reason, plan or conclusion to reach it. Use this order.

```markdown
# [Event name from the event record] trip report

[1 sentence: the event name, dates and place from the event record. Nothing else: no sentence about their focus or main takeaway unless they said it. Without an event record, use the event name they gave and leave out dates and place.]

## Sessions that mattered

### [Title] ([code], [type], [day])
[2 or 3 sentences on what it covered, from the abstract (when you have one) and the attendee's notes. With neither, write no sentence about what it covered, and do not infer content from the title. Then one sentence on why it mattered, in the attendee's words. If they gave no reason, leave that sentence out.]

## Themes
[What the attendee noticed across sessions.]

## New launches
[Each launch: AWS's name for it, its status and date, and a link. Add why it matters to the attendee's work only in their words. If they gave no reason, leave it out and ask.]

## Learning, connections and next steps
[People they met, resources they will use, follow-ups they agreed, and what they will do in their first week back.]

[Optional. End on the attendee's own first-week plan, in their words. Write no other closing sentence: no verdict on the week and no lesson they did not state.]
```

Leave out any section the attendee gave you nothing for. Do not pad it.

## Facts

- Use session titles and codes exactly as the API gave them. Put the code in the report so a reader can find the recording after the event.
- Paraphrase abstracts. Do not copy them whole.
- Show times in the event's local time with the timezone name (America/Los_Angeles for re:Invent), because attendees travel from many timezones.
- Give recordings no link or date unless the attendee has one. AWS says recordings and slide PDFs will be available on demand after the event.
- A claim is never stronger than its source. If a session abstract says "explore", do not write "proved".
- Stay inside what they said. If they said "I want to try adding an audit log to our support agent", write that and stop. Do not add what the log is for or what it will show. If the reason matters, ask for it.
- Say nothing in the report about what you could or could not check. That goes in chat.
- Add no connecting reasoning. Phrases such as "so we can", "which means", "that gives us" and "for our team" introduce a reason the attendee has not given. Use them only when the attendee wrote them.

## Voice

First person, plain, specific, matter-of-fact. Name the real thing: the service, the number, the constraint, what broke. Lead with the problem a session helped with, and what the attendee will do about it. Show a process as numbered steps only when the attendee described one. Do not address readers as "developers". No hype and no puffery, and no stack of adjectives where one fact would do.

Write the way the attendee would say it to a colleague over lunch. Vary the sentence length. Skip announcements before a point ("Here's the thing"), summaries of what you just said, and aphorisms at the end of a paragraph.

## Words to flag

Flag these words to the attendee instead of rewriting them silently, and apply the check to the attendee's own prose only. Never change a session title, a service name or a product name. Real names collide with the list: "AWS Transform" contains "transform" and "AgentCore Harness" contains "harness".

dive deep, deep dive, delve, journey, supercharge, embrace, burning question, unlock, roadmap, uplevel, future-proof, revolutionize, navigate, harness, transform, drives, realm, vibrant, innovation, empower, foster, elevate, in summary, in conclusion, testament, unleash, trenches, distilled, spearhead, revolution, landscape, imagine this, master, treasure trove, masterclass, ninja, superhero, battle, gamechanger, warrior, game changing, wizardry, wizard, wizards, extraordinaire, level up, drowning, strangle, game-changer, union, groundbreaking, copilot, insanely, arsenal, calling all, ultimate solution, turbocharge, boasts, unprecedented, supremacy, avengers, fight, weapon, conquer, stupid, crazy, genie, army, no sweat, veritable, fam, co-pilot, sidekick, powerhouse, turbocharged, turbocharges, pivotal, my friends, nirvana, armed, possibilities, witnessed, as developers, game, awesome sauce, craving, power, sexy, forsee, emerge

Also avoid fantastical or exaggerated descriptions such as "magic" or "revolutionary", and anything that makes a tool sound more powerful or easier than the session showed.

## Never in the report

- Personal time entries, or anything the attendee did not say.
- Tokens or sign-in details.
- Content from a restricted program (a session with an `experiences` value) unless the attendee agreed.
- An opinion the attendee did not give. If a section needs one, ask for it.

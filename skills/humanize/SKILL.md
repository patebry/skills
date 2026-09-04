---
name: humanize
description: Rewrite an outbound message to a person (Telegram, Slack, email, a PR or issue comment, a customer or partner update) so it reads as written by a human rather than generated, and cut it to the length it deserves. Not for code comments, documentation, README or spec prose, commit messages, or the assistant's own replies in the terminal; those keep their own conventions. Not for translating, proofreading for grammar only, or drafting a message from scratch with no source text. Use when a draft exists and reads stiff, padded, over-structured, or machine-written, and a real person is about to receive it. Triggers on "humanize", "make this sound human", "cut this message in half", "remove the em dashes", "make it less AI", "just give me a quick message", "shorten this before I send it".
---

# humanize

Take a drafted message and return the version a competent person would have written to that reader.

Two things are wrong with most machine-written messages: the shape and the register. Fix the shape first. Length falls out of the shape.

## Scope

This is for messages a person receives and reads once: chat, email, a comment on a pull request or issue, a customer or partner update, a status note to a team.

It is not for code comments, documentation, commit messages, or the assistant's own terminal replies. Those are read by different people for different reasons and have their own conventions. Do not apply this skill to them.

## What gives a message away

These are structural, and they survive any amount of word-level polishing. Name them, then cut them.

**Throat-clearing before the ask.** The message opens with context, background, or a statement that a message is being sent. The reader has to reach paragraph three to learn what they must do. The ask belongs in the first two sentences.

**Scaffolding on a short message.** Section headers, bold labels, and horizontal rules on something three paragraphs long. A chat message does not need headers. If the whole thing fits on one screen, it is prose.

**Lists where prose would do.** A numbered list implies its items are parallel and countable. Three sentences about three unrelated things are not a list. Reserve lists for genuinely parallel items the reader will scan or tick off.

**The same point at two lengths.** A summary sentence, then the same content expanded underneath. One of them is redundant, and it is almost always the summary.

**An unrequested closing offer.** "Happy to jump on a call", "let me know if you want me to dig deeper", "I can put together a doc if that helps." Nobody asked. Cut it unless the offer is the actual point of the message.

**Signposting phrases.** "I wanted to reach out", "just circling back", "as mentioned above", "please don't hesitate to", "I hope this finds you well." They announce the message instead of delivering it.

**Hedging stacks.** "It might potentially be worth considering whether we should perhaps." One hedge is honest. Three is evasion. Pick the single qualifier that carries the real uncertainty, unless the stack was drafted deliberately: on the messages under "Serious messages" below, a hedge may be doing legal work, so ask before collapsing it.

**Em dashes as the default connector.** More than one in a short message reads as generated. Related tells in the same family: semicolons in casual chat, "not just X but Y", the rule-of-three cadence ("faster, cleaner, cheaper"), and sentences opening with "Importantly," or "Notably,".

## What to do instead

**Lead with the action or the news.** What must the reader do, or what changed? That is sentence one. Context comes after, and only the context needed to act.

**One idea per paragraph, two to four sentences.** A paragraph doing two jobs is two paragraphs.

**Prefer a period.** An em dash, a comma splice, or a colon is usually a sentence boundary in disguise. Write the two sentences. The exception is a genuine aside or appositive, where the dash sets off a phrase that could not stand alone; one of those per message is fine.

**Use contractions, and the words the reader would say out loud.** "We can't ship Friday" beats "we will be unable to ship on Friday." Read the draft aloud; anything you would not say, rewrite.

**Name the concrete thing.** The number, the file, the date, the person. "The import fails on rows with a null email, about 40 of 900" beats "there are some data quality issues."

**Cut every sentence that only announces the next one.** "There are two things to flag here." Then flag them.

**Say the risk plainly.** Especially when the news is bad or the mistake was ours. "We missed this in review and it went to production Tuesday" is better received than any softened version, and it is shorter. This stops at the line drawn under "Serious messages": state the facts and the impact, but never volunteer fault the draft did not admit.

**Keep the precision.** Humanizing is a register change, not a loss of accuracy. Never trade away a number, a name, a version, or a caveat to sound casual. If a sentence is long because the fact is complicated, it stays long.

## Line-level substitutions

The same handful of swaps carry most of the work. Apply them by reflex.

"I wanted to let you know that X" becomes "X."

"We are currently in the process of investigating" becomes "we're looking into it."

"There appear to be some issues with" becomes the issue, named.

"Please find attached" becomes "attached is" or nothing.

"At your earliest convenience" becomes a date.

"Going forward" and "moving forward" become nothing, or "from now on" when the contrast matters.

"Reach out" becomes "ask", "email", "call", "message" — whichever one you mean.

"Leverage", "utilize", "facilitate" become "use", "use", "help".

"In order to" becomes "to".

## What not to change

Rewriting is licensed on form, not on substance.

**Do not invent facts.** If the draft is vague because the writer does not know the number, leave it vague and flag it back to them. Making the message concrete by guessing is the worst possible outcome of this skill.

**Do not soften or harden a commitment.** "We should be able to" and "we will" are different promises. Keep whichever the writer chose.

**Do not drop a caveat, an exception, or a name.** If a clause exists to protect the writer, it survives the rewrite.

**Do not add warmth the writer did not have.** An apology, a compliment, or an emoji you introduced is a claim about their state of mind.

**Do not change the decision.** If the draft says no, the rewrite says no, faster.

When a cut would lose one of these, keep the sentence and cut somewhere else.

## Cutting it in half

Halving removes whole sentences. It does not compress every sentence into a fragment. A message of clipped stubs reads as machine-written in a different way.

Work in this order and stop when you hit the target:

1. Delete the opening until the first sentence that carries information.
2. Delete the closing offer and the sign-off filler.
3. Find the restated point and keep the longer, more concrete version. Delete the summary.
4. Replace any list of three or fewer items with a sentence.
5. Delete headers, bold labels, and rules.
6. Merge paragraphs that make the same point from two angles.
7. Only now, tighten individual sentences.

If it is still too long after step 7, the message is carrying more than one topic. Split it or ask which topic matters.

## Matching the channel

Register follows the channel and the relationship, not a single house voice.

A Telegram or Slack note to a colleague is loose: lowercase openings, no salutation, fragments where they read naturally, one thought per message. An email to a partner or customer keeps a greeting and complete sentences but still leads with the point. A PR or issue comment is technical and terse: what is wrong, where, what you want done.

Match the reader's own register when you have their prior messages. If they write in three-word lines, do not send them five paragraphs.

## Serious messages

Legal, financial, security, incident, and contractual messages, and anything a third party might read later, do not get loosened. Plain and precise is the goal there and casual is a liability.

Strip the padding, keep every number and qualifier, and never add warmth that implies the situation is lighter than it is. Leave hedges that look deliberate and ask about them instead. Do not concede fault, liability, or a cause the draft did not state.

## Workflow

Establish the channel and the audience. If the draft does not make either obvious, ask in one line before rewriting; the wrong register wastes the whole pass. If it is legal, financial, security, incident, or contractual, the section above governs the whole rewrite. Then structure, then register:

1. **Structure.** What is the ask, and is it first? What can be deleted entirely? Does anything need to become prose, or prose become a list? Decide the shape before touching wording.
2. **Register.** Line by line: contractions, em dashes, signposting, hedges, vague nouns. Read it aloud.

Return only the rewritten message, ready to paste. No preamble, no explanation of what you changed, no list of the tells you found. Three things may accompany it, nothing else: the labelled versions when the user asked for options, one line saying the draft is already good when it is, and one line asking for a fact the draft is missing. A rewrite that only shuffles words is worse than no rewrite.

When the user asks for it on the clipboard, pipe it through `pbcopy` on macOS, and still show the message in the reply so they can read it before sending.

## Check before returning

Read the rewrite once as the recipient. Then confirm:

- The first two sentences say what happened or what to do.
- Every number, name, date and caveat from the draft is still present, or was deliberately cut with the writer's knowledge.
- Every number, name and date in the rewrite traces to the draft or to something the writer confirmed. Nothing was supplied to fill a gap.
- No header, bold label, or rule survives on a message that fits one screen.
- No more than one em dash, and it earns its place.
- Nothing offers help nobody asked for.
- Read aloud, no sentence makes you stumble.
- On a serious message, nothing was loosened, conceded, or warmed.

If the message would embarrass the writer when forwarded, it is not finished.

## Anti-patterns

**Rewriting into your own voice.** The goal is the writer sounding like themselves on a good day, not sounding like you. Their voice is what recurs across the draft and their prior messages: sentence length, whether they use contractions, how they open, how they say no. Where their habits and the reader's register pull apart, formality follows the reader and word choice follows the writer; you can be terse and still write "Dear Ms Vance."

**Treating this as a grammar pass.** If nothing was deleted, nothing was humanized.

## Example

Before:

> Hi team, I wanted to reach out regarding the status of the reporting migration that we discussed last week. As you may recall, we had originally planned to complete the cutover by the end of this month. Unfortunately, we've encountered some unexpected challenges with the data validation layer — specifically, we've discovered that a subset of historical records do not conform to the expected schema. As a result, we are now anticipating that the timeline may need to be extended. We're currently evaluating our options and will circle back with a revised plan. Please don't hesitate to reach out if you have any questions or concerns in the meantime.

After:

> The reporting cutover is slipping past the end of the month. Some historical records don't match the expected schema, which is holding up data validation. We're weighing options and we'll come back with a revised plan.

Down to a third of the length: 109 words to 36. It leads with the news, drops the greeting, the recap, and the closing offer, and keeps the writer's promise at the strength they made it. It still says "some records" and "a revised plan", because the draft never said how many or when. Guessing there would have been the worse message, however much better it read.

The gap goes back to the writer instead, in one line beside the rewrite:

> How many records, and when will you know the new date? Both would land better, if you have them.

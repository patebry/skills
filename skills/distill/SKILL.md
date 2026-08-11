---
name: distill
description: Turn a body of source material into a short, defensible document. Not for reviewing code, diffs or pull requests, tasks ending in a code change, or exploring and documenting a codebase — use code-review, security-review, simplify or init. Not for a single short document or a straight summary. Use for contract bundles, RFPs, spec bundles, research corpora, interview transcripts, incident evidence — dense material whose correctness cannot be assumed, where the output will be acted on and being wrong is expensive. Triggers on "distill", "synthesise these", "what do these documents actually say", "produce a memo from this", "verify this memo against its source corpus".
---

# distill

Produce a short document from a body of material, built to hold up when someone checks it.

Optimise against confident wrongness: misquotes, miscounts, and claims no source makes.

## Gate

Run this when all three hold:

- The material is substantial and its correctness cannot be assumed.
- Someone will act on the output or read it critically.
- Being wrong costs more than being slow.

Otherwise read the material and answer.

---

## The arc

```
0  Frame the material and build the source register
1  Ask the human for what no source contains
2  Read in parallel over disjoint slices
3  Synthesise across the readers
4  Draft
5  Show the human, for shape only
6  Verify the load-bearing claims, before rewriting
7  Rewrite
8  Attack in rounds until clean or capped
9  Verify the artifact the reader receives
```

**Never skip 1, 5, 6 or 8.** Step 5 runs with a human when one responds, and with its stated substitute when none does.

**Adjudicate every agent finding against its cited span before applying it.** Reviewers are sometimes confidently wrong, and may talk you into retracting something true. Where two disagree and the span is ambiguous, record the ambiguity in the deliverable rather than picking. This applies at steps 3, 6 and 8.

**Every agent brief in steps 2, 3, 6 and 8 carries three closing elements:** the source register, "your final message IS the deliverable, written for an orchestrator not a human", and the terminal line `===RESULT=== <DONE|BLOCKED> — <one-sentence summary>`.

**If the material arrived in conversation rather than on disk**, pointers become source label plus a verbatim locator string, and "file" means whatever durable artifact you have — a written file if you can write one, otherwise one consolidated message you maintain.

---

### Step 0 — Frame the material and build the source register

Read any orientation material first and alone — cover note, README, the "read this first". It states what is authoritative, what is already known to be wrong, and what is out of scope. If none exists, build an index in one cheap pass; that index becomes the spine in step 2.

Write a **source register**, one row per source: identifier, what it is, length, version, date, authority rank, confidential yes/no. State where it lives. It is pasted into the briefs named above.

Head it with one line — **Authority model: hierarchical | peer | mixed.** Authority rank is one of:

- **governing** / **governed-by:\<id\>** — amendments beat the contract body; addenda beat the RFP; code beats README; an erratum beats the paper; raw logs beat a write-up.
- **peer** — interviews, survey responses, independent incident reports. For peer or mixed corpora, state "no source governs among the peer set". **Peer disagreement is data, not error:** carry every position to step 3, which reports the split rather than resolving it. **Peer corpora always take step 2's identical extraction list** — split sizes cannot be recovered afterwards.
- **secondary** — restates someone else's evidence (vendor decks, analyst claims, summaries). Step 6 treats these differently.

### Step 1 — Ask the human for what no source contains

Identify what cannot be answered from the material at any depth — facts about the reader's own organisation, capacity, history, constraints or intent. Ask before investing in reading.

- Use a structured question tool if available (e.g. AskUserQuestion); otherwise ask in plain text as one numbered list and stop for the answer.
- 2–4 concrete options per question, recommendation first. Include an option letting the human hand you raw facts instead of picking from your guesses.
- Batch them. Two rounds of four beats eight rounds of one.

**If no human responds in this run, or answers only some questions:** put the unanswered ones in the deliverable as "Assumptions, and who must confirm them", proceed on stated assumptions, and mark every conclusion depending on one.

### Step 2 — Read in parallel over disjoint slices

**Reader count:** take the low number in the Sizing cell. Add readers so that no slice exceeds one agent's close reading — roughly **50 pages of dense prose, ~40k words, or 8 short sources**, whichever binds first. Sources you query rather than read end-to-end — logs, databases, dashboards, large event stores — do not count toward the ceiling; each counts as one short source, and its brief carries the queries to run rather than a page range. State the ceiling you used. Take the cell's upper number instead when the material is unfamiliar. Group small sources together; split large ones.

Every reader also receives the **shared spine**: whatever any reader needs to interpret any slice. Definitions, table of contents, amendment list, glossary, the orienting document — and for corpora with none of those, the step 0 index, the source register, the common instrument (interview guide, screener, questionnaire), and **any single source the findings are read against** — a competitor teardown, a benchmark, a prior report. If the spine is genuinely empty, record "spine: none" in the brief. Disjointness governs what you report on, not what you may read.

Every brief contains:

- Its slice, and a note that other agents cover the rest.
- A **numbered extraction list** — specific, not "summarise". **Use an identical list across all readers unless you can state affirmatively that the deliverable will contain no count, proportion or "N of M" claim.** Each reader answers every item explicitly, including "not present in my slice". It cannot be added retroactively without re-reading the corpus; using it unnecessarily costs one line per reader.
- **A reproducible pointer for every load-bearing finding**, plus the verbatim span where the material is text: prose → document + clause + page; code → `path:line` at a commit; logs → query + window + filters; interviews → transcript ID + timestamp; papers → DOI + section. For counts and absence, return the enumeration or the search you ran.
- **"List what you looked for in your slice and did not find."**
- A flag for where the source contradicts itself or the brief's assumptions.
- An output budget — findings cap, word cap, verbatim spans one sentence unless the wording is itself the finding. Step 3 must fit all of it in one context.
- Confidentiality constraints from the register.

Confirm any file path you name exists before spawning.

**If parallel agents are unavailable:** read the slices yourself in sequence, writing each slice's notes out before starting the next.

### Step 3 — Synthesise across the readers

Disjoint reading means nobody saw the whole. Cross-source findings surface here or, at best, late in step 8.

Give one agent all readers' notes — not the raw sources to read in bulk, but with authority to run targeted searches against any pointer — tasked only with what no single reader could see:

- **Recurrence and counts.** These are counts *of reader answers*: report each with the denominator of readers who answered, then cross-check against a targeted search of the raw sources before anything enters the draft.
- **Conflicts between sources** — which governs under the register's authority ranks; or, for peers, the n-way split, each position with its count and denominator.
- **Self-contradiction and assumption breaks** — collect every flag readers raised, deduplicate across slices, and carry each forward as a drafted finding or a stated ambiguity. For a single-source corpus this is the main conflict category.
- **Gaps** every reader looked for and none found.
- **Chains** — couplings, dependencies, timelines crossing slice boundaries.

Each output carries the source identifiers it aggregated.

**If the notes exceed one context:** group readers 2–3 at a time (an odd reader joins a group of three) into partial syntheses, recursing if the partials still overflow. Each partial passes through every reader's raw answers to the recurrence items verbatim; the final synthesis recounts from those, never from a partial's totals. Prevent this where you can by tightening step 2's output budgets.

### Step 4 — Draft

Merge notes and synthesis into one file; state its path. If the document already exists and the request is to verify it, adopt it unchanged as this draft and build the claim map from it, recording the reader column as "pre-existing" for claims no reader returned.

- **Every claim traces to a reader's note or the synthesis.** Inventing connective facts is a main route by which hallucination enters. A fact you need but nobody found is a gap — write it as one.
- **Recompute derived numbers from source values**, never from a previous draft; otherwise an arithmetic error can survive every rewrite.

Build the **claim map** beside the draft — claim ID, claim as drafted, pointer, verbatim span, reader who returned it, verification status (unchecked / TRUE / FALSE / PARTLY / unverifiable). State its path. It is an input to steps 6 and 8, and is kept out of the artifact at step 9.

### Step 5 — Show the human, for shape only

Tests **scope, emphasis and structure** — not content. Expect: too long, wrong emphasis, answering questions nobody asked.

Mark the draft unverified and mark every number and quotation `[unverified]`. This is not licence to hand over content you believe is wrong.

Stop and wait. **If no human responds in this run:** give a fresh agent only the original request and the draft, asking *"what in here does not answer the question that was asked?"*, and record in the final report that no human review occurred.

### Step 6 — Verify the load-bearing claims, before rewriting

Enumerate every number, date, quotation, named commitment and causal claim in the draft — **all of them, no filtering.** You are a poor judge of which of your own claims carry weight. Order the list most-consequential first, so a truncated pass still covers what matters.

**Partition the claim list by the sources its pointers name**, so each verifier holds claims resting on one coherent set of sources. Use the reader count the Sizing cell gave step 2; if there are fewer source sets than that, split the largest set's claims by consequence order.

Each verifier receives its claims, their claim-map rows, and **access to the primary sources those pointers name.** Each returns `TRUE / FALSE / PARTLY / unverifiable` with a verbatim span and pointer.

**Verify two distinct things:**

1. **Fidelity** — does the document match the source?
2. **Truth** — is the source right? A stale README, a lying comment, a broken metric, a retracted paper, a misremembered write-up all pass a fidelity check and are still wrong.

Any claim resting only on a **secondary** source must be corroborated against primary evidence, or **attributed in the deliverable**: "the runbook states X", not "X".

Write every verdict and span back into the claim map. Then: **FALSE** → correct against the source or cut, and carry the correction into step 8. **PARTLY** → keep and mark inline. **Unverifiable** → attribute or cut.

Verify before rewriting; verifying after means rewriting twice in the common case.

### Step 7 — Rewrite

Short, structured to the question asked, in the order asked. Cut anything that shows your work instead of delivering the answer.

- **Quote verbatim or do not quote.** Truncating mid-clause, especially where the dropped clause is inconvenient, is the failure reviewers punish hardest.
- **Strip step 5's blanket `[unverified]` markers.** The only marks that survive are step 6's PARTLY and unverifiable outcomes, marked inline. Say which figures are judgment rather than measurement.
- **Separate "the source says X" from "I conclude X".**
- **Record material absences** in their own section.
- **Update the claim map as you rewrite** — new claims get rows, cut claims are deleted, corrected claims get corrected text. A stale map silently exempts every changed claim from step 8.
- If the question rests on a false premise or cannot be answered from any available source, **saying so is the deliverable.** Do not manufacture an answer to a malformed question.

### Step 8 — Attack in rounds until clean or capped

**Use close to this wording:**

> Your job is to FIND ERRORS, not confirm correctness. Assume the document is wrong until proven otherwise. Report ONLY defects: what the document says, what the source says, and the exact correction. Style, tone, and "could be clearer" are NOT defects. Returning zero findings is a valid result for a section that is correct — do not manufacture findings to demonstrate effort.

Confirmatory framing produces far fewer findings; keep the wording adversarial.

**Each round:**

- Split by **defect class**, not by section — each needs a different reading mode, and hunting one blinds you to the others. Choose 2–4 from how *this* deliverable could be wrong. Worked examples: arithmetic and internal consistency; quotations and source attribution; recurrence-count integrity and strength of generalisation (interview and survey corpora); attribution of secondary material restated as fact; timeline and causal attribution (incidents); does-the-code-do-what-the-doc-claims (codebases); cross-reference integrity of defined terms and clause numbers (contracts); methodology and strength of evidence (research).
- **Give every attack agent the claim map** and point it at the **primary sources**. Disjointness governs *reading* in step 2; attack agents share sources and split by defect class. Where the corpus exceeds one context they work from the map and cited spans, with authority to pull the full source at any pointer they doubt.
- Require exact corrections, not observations.
- Tell later rounds what earlier rounds fixed, and that **fixes break neighbouring text** — that is where the remaining defects are.
- **After applying a round's corrections, update the claim map before the next round** — new rows for new claims, deletions for cut ones, corrected text and reset status for changed ones. Same rule as step 7; a stale map exempts every claim you just changed.
- **If the deliverable has a rendered form, render it from the Sizing minimum onward** and give every round after that the rendered output as well as the source. You cannot know in advance which round is the last.

**Classify every finding: conclusion-changing / factual / cosmetic.**

**Termination.** Run the Sizing minimum in full even if an early round returns nothing. After that, continue while any round returns conclusion-changing or factual findings. Stop at the first round returning none, or at the top of the Sizing range, or at 6 rounds — whichever comes first.

**If the count is not falling**, diagnose before patching:

| Cause | Remedy |
|---|---|
| Long document, reviewers keep finding new surface | Cut length; narrow each agent's slice |
| Defect definition too broad, style being counted | Restate the exclusion; re-run |
| Source genuinely ambiguous | State the ambiguity in the document; stop resolving it |
| One reviewer generating most findings | Re-anchor it to sources; check its findings before acting |
| Section structurally wrong | Stop patching; rewrite it |

### Step 9 — Verify the artifact the reader receives

Check the bytes that reach the reader, not your source file.

- **Rendered output** (PDF, slides, published page, export): if a prior round saw it rendered, extract its content back out and diff against the source; if no round did, attack it here before diffing. Expect whitespace and ligature noise — review by eye rather than requiring an empty diff. Check structure as well as words: code blocks reflowing into prose, tables collapsing, headings lost.
- **Plain text**: read it once as it will be received.

A correction can land in the source and never reach the file the reader opens. Confirm no claim map, claim IDs, `[unverified]` markers or reviewer annotations survive in the artifact.

---

## Sizing

Rows describe **what happens if the deliverable is wrong**, not who wrote the sources. External or adversarial means a counterparty, regulator or opponent reads the output or exploits the error, or the output commits money or legal obligation — a pre-signature contract memo is this row even though it never leaves the building.

| | Small (up to ~40pp **and** few files) | Large (~40pp+ **or** many files) |
|---|---|---|
| **Wrong is embarrassing** | 1–2 readers, 1–2 rounds | 2–3 readers, 2 rounds |
| **Someone acts on it** | 2–3 readers, 2 rounds | 4–6 readers, 2–4 rounds |
| **External or adversarial** | 3–4 readers, 3–6 rounds | 5–8 readers, 3–6 rounds |

Round up when between cells. Within a cell take the low reader number and add per step 2's ceiling; take the upper number when the material is unfamiliar. Steps 1, 5, 6 and 8 run at every size; what scales is the number of agents and rounds.

## Anti-patterns

- **Reading everything yourself first** — you run out of context and summarise a summary. Exception: step 2's no-parallel-agents fallback.
- **Asking agents to "see if it looks right."** Exception: step 5's shape-only substitute, which is deliberately open-ended and never touches content.
- **Padding with everything you found.** Material discovered but unused is not a reason to include it.

## Reporting back

- Findings by severity, not raw counts — counts get gamed at both ends.
- What changed a conclusion, not what changed a typo.
- **What you got wrong**, if it would have reached them.
- What remains unverified or unfound, and who owns closing it.
- Whether you hit the round cap, and what is still open.

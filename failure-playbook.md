# Failure Playbook: where Gemini 3.8 Flash really breaks

Real failures from graded simplified runs on **Gemini 3.8 Flash (Low, Workspace)**, Nectarbloom i18n Prompt and Rubrics. Each technique shows its scale in the graded runs, what the model actually does, and how to build the same pressure into your own item.

The companion web page is `failure-playbook.html`.

## The data

- **69 valid runs** used, of 86 per-task runs.
- **~936 Fail ratings** analysed.
- **17 excluded**: no access, invented data, or record and run integrity problems (not legitimate stumps).
- **5 locales**: fr, de, en, ko, ja.
- **Lowest gate 0/54** (the best stump). Gate = Pass weight divided by total weight on the first simplified run.

About **80% of all Fails** come from the model **not producing the Workspace deliverable**. Content errors are about **15%**, but they are the cleanest stumps. Counts are Fails, not tasks, so atomic criteria inflate the artifact-missing categories; every count is about plus or minus 10%.

## Nine ways it fails, in rank order

### 1. One-action stop (multi-deliverable dump with an email draft)
**~305 Fails, 29 tasks, mean weight 2.2. Biggest single cause.**
The model makes one Gmail or Calendar card, replies only "I drafted this for you.", and drops every other ask (the analysis, the Doc, the Sheet, the other drafts).
**Build it:** one dump that asks for computed answers plus two or more drafts to different people plus a Doc or Sheet. Strongest in ja.

### 2. Unwritable file (edit or create a file the connector cannot write)
**~255 Fails printed in chat, ~126 tool-limit Fails, 43 tasks. Cheapest stump.**
The model prints the file in chat with no save, or returns an explicit tool-limit message. One missing file fails 5 to 15 atomic children.
**Build it:** ask it to edit an existing .xlsx, .csv, .md or PDF in place; make a .png, PDF or .xlsx with an exact name in a folder; delete or move Drive files; create a Gmail label; or add a worksheet to an uploaded .xlsx. QC accepts these as valid failures.

### 3. Implicit epistemic status (open vs settled, verified vs claimed)
**19 Fails, 7 tasks.**
The model calls things done or answered when the sources leave them open, or invents an approval (for example a handover marked signed although the protocol is blank).
**Build it:** two documents that disagree, or a step that has not happened yet, which the user does not flag.

### 4. Complete enumeration across many emails or files
**23 Fails, 5 tasks.**
The model reports a subset (5 of 8 alerts; 4 of 10 people; one month dropped from a range).
**Build it:** scatter about 8 to 15 records over Gmail and Drive, some in a different thread or format, and require all of them.

### 5. Long chains of derived arithmetic
**17 Fails, 4 tasks, mean weight 2.6 (highest).**
One wrong input spreads through many metrics (308 h instead of 318.5 h cascaded through 14 metrics).
**Build it:** a figure that must be read correctly, then reused across many computed outputs.

### 6. Buried detail in a thread or note
**~46 Fails, 23 tasks, low weight 1.8.**
The summary is right but misses one dated or conditional fact (a "may decline" remark, a specific date, "3 days late").
**Build it:** one load-bearing detail buried in a long thread or note.

### 7. Social or implicit constraint taken from another source
**5 Fails, 4 tasks, weight 2.4.**
The model ignores an unstated norm (uses data the source rules out; replies formally to a casual message; asks all 10 instead of the 7 with clips).
**Build it:** put a norm, scope or register in the files or the tone, not in the explicit ask.

### 8. Null answer or "already correct"
**Content stump.**
The model "fixes" something already right (corrects accurate timestamps by minus 1 hour; invents an invoice for a month that has none).
**Build it:** a case where the correct action is to change nothing, or to report that nothing applies.

### 9. Smaller levers
- **General knowledge over the user's own document** (6 Fails): 30 to 50% instead of the document's 60%.
- **Placeholder or entity variant** (~11 Fails): `ploverandpine@example.com`; `jobooth72` vs `jo.booth72`.
- **Hedged range instead of a committed figure.**
- **Time zone of the demo account** copied into a local time.

## What does not stump it

Conditional pass rates (Pass divided by Pass plus content Fail, when the artifact existed). Do not lean on these as your stump.

| Ask | Conditional pass rate |
|---|---|
| Leave unsent | 100% (32/32) |
| Correct recipient | 88% |
| Exclusion or "only" constraints | 88% |
| Confidential or unshared | 85% |
| Calendar event created right | 83% |
| Output language | 82% |
| Dates | 75% |
| Numbers | 70% |
| Open or unverified status | 65% |
| Register | 62% |

Single-deliverable analytical reports in chat pass easily (76 to 98%). If your whole item is one report in chat, it will not stump the model.

## QC traps: what gets a stump sent back

- **Values no upload holds.** Every exact figure must trace to an uploaded file.
- **Inputs not uploaded.**
- **Unasked criteria**, often lifted from Not Known, while explicit asks have no criterion.
- **Invalid run**: a single turn that copies the opener, a drip-fed or steered chat, or an unrecorded steering turn.
- **Uploads that do not match the run**: an output the run never produced, a previous run's PDF used as input, or a turn record edited after the run.
- **Grading claims instead of state.** The Gmail card is a compose preview; grade cards from the share page, never from the chat PDF placeholder.
- **Time-zone display artefacts.** A demo account in IST, GMT+03 or US time is not an error by itself.
- **Gaming the gate.** Cutting criteria, inverting weights or misgrading to get under 40%. Harden the item instead.

## By locale

| Locale | Valid tasks | Mean gate | Main failure mode |
|---|---|---|---|
| fr | 11 | 28.9% (lowest) | File edits and tool limit |
| de | 16 | 37.0% | Status judgement and one-draft runs |
| en | 11 | 38.5% | Content errors |
| ko | 9 | 38.5% | One-action stop and printed tables |
| ja | 18 | 42.5% | One-action stop |

Small-n and confounded by universe and contributor.

## The rule

**A stump only counts if a careful agent would have passed.** Same prompt, same inputs, same tools: the evidence was reachable, the request was clear, and nothing was hidden, broken or forced. If a careful agent would also have failed, it is not a valid stump. Harden the item; never game the gate.

## Caveats

Heuristic classification (regex plus hand tagging of about 140 content Fails), n = 69, 2 to 5 universes per locale, and many tasks per contributor. The rubrics and gates reflect our fixed grading under the content-evidence rule, not the contributors' original grades.

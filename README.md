# The Unofficial Guide

Yudhiishbala V Senthilkumar — Corpus: campus_life

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

This project answers questions about housing, dining, courses, and campus
policies using 88 campus-life documents. It retrieves relevant passages
and uses Gemini to answer with source filenames. The starter produced
88 chunks averaging 317 characters, with lengths ranging from 178 to 549
characters. Its first successful answer explained the housing lottery
and cited admin_housing_lottery.txt.

## Chunking Strategy

**Chunk size:** One complete document; currently 178–549 characters.
**Overlap:** 0 characters.

I chose document boundaries instead of fixed character windows because
campus_life contains short posts about a named course, building, or policy.
Keeping each post together preserves the heading and the context needed
to interpret its facts. Splitting these short posts could separate a
price or exam rule from the subject it describes.

My chunker emits one chunk per nonempty document. There is no fixed
character limit, so this strategy would need reconsideration for longer
documents. The starter also produced 88 chunks; I expect the same count
because preserving complete posts is an intentional choice.

## Sample Chunks

All five sampled chunks preserve complete sentences and identify their
subject. The BIOL 160 document contains an odd "I lived here" sentence;
the chunker preserves the source text rather than correcting it.

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```text
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_biol_160.txt#0` — produced by: `chunker.py::split_documents`

```text
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.
```

**Chunk 3** — source: `course_hist_118_workload.txt#0` — produced by: `chunker.py::split_documents`

```text
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** — source: `dining_pellew_dining_hall_followup.txt#0` — produced by: `chunker.py::split_documents`

```text
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_innisfree_hall.txt#0` — produced by: `chunker.py::split_documents`

```text
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.
```

## Sample Answer

**Question:** Are the CS 340 midterm and final open-book or closed-book?

**Answer:**

```text
The CS 340 midterm and final are both open-book.

Sources: `course_cs_340_exams.txt` and `course_cs_340.txt`

Sources retrieved: course_cs_210.txt, course_cs_210_exams.txt, course_cs_340.txt, course_cs_340_exams.txt, money_textbooks.txt
```

This answer was collected with the original 0.60 cutoff. I retained the
starter's grounding instruction because it requires using only supplied
documents, refusing unsupported answers, and naming source filenames.
The sample correctly distinguishes CS 340 from the CS 210 passages
also retrieved.

**My relevance cutoff:** 0.69
**Top-k:** 5

The five in-corpus questions had best distances from 0.2442 to 0.5583.
The five out-of-scope questions ranged from 0.8246 to 0.9340.
I chose 0.69, approximately halfway between the highest in-corpus
distance and the lowest out-of-scope distance.

This cutoff separates all ten measured questions. The original 0.60
also separates them, but 0.69 provides more room for relevant questions
with different wording. That also risks admitting unrelated questions
whose distances fall between 0.60 and 0.69. These ten examples do not
guarantee performance on new questions.

| Question | In corpus? | Best distance |
|---|---|---|
| What signature is required to withdraw from a course? | Yes | 0.5583 |
| How much does one wash cycle cost in Morrow House? | Yes | 0.2442 |
| Are the CS 340 midterm and final open-book or closed-book? | Yes | 0.4418 |
| How many hours per week outside class does ECON 101 require? | Yes | 0.3249 |
| Which midterm score is dropped in STAT 150? | Yes | 0.3283 |
| What is the capital of Mongolia? | No | 0.8246 |
| How do I change the oil in a diesel engine? | No | 0.9340 |
| Who won the 1994 World Cup? | No | 0.8859 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.8442 |
| How do I write a for loop in Rust? | No | 0.8960 |

## How I Used AI

**1. Understanding the Python environment.**
I used Codex to learn how a virtual environment separates project
dependencies from the system Python. When my environment check failed,
it explained the Python version mismatch and guided me through rebuilding
the environment with Python 3.13. I ran the commands and shared the output
to verify that all ten checks passed.

**2. Understanding chunking and retrieval.**
I used Codex's explanations and drafted examples—including questions,
criteria, and chunker code—to learn how the pipeline works. Keeping short
posts together showed how a chunk can preserve both a fact and its context.
Comparing retrieval distances helped me understand the cutoff tradeoff:
a lower threshold can reject relevant questions, while a higher one can
admit unrelated questions. I applied a 0.69 cutoff and checked the output
to confirm that the relevant question passed and the unrelated question
was refused.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->

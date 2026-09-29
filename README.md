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

## Run Log — Before

Raw evidence: [results/run_2026-09-29_1531_before.md](results/run_2026-09-29_1531_before.md). `run_eval.py::main` made three uncached model calls per in-corpus question on 2026-09-29. I inspected each answer against the original corpus documents. Retrieval and chunking are deterministic, so their measurements repeat in each column. The gate was checked once per out-of-scope question, as specified by `run_eval.py::check_out_of_scope`.

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Top five retrieved chunks contain the answer | at least 4/5 questions | 5/5 | 5/5 | 5/5 | MET |
| 2. Answer text names a source filename | 5/5 questions | 5/5 | 5/5 | 5/5 | MET |
| 3. Gate refuses out-of-corpus questions | at least 4/5 questions | 5/5 | 5/5 | 5/5 | MET |
| 4. Sampled chunks preserve complete, identified information | at least 4/5 chunks | 5/5 | 5/5 | 5/5 | MET |
| 5. Answers state the correct facts without contradictions | at least 4/5 questions | 5/5 | 5/5 | 5/5 | MET |

### Actual output used to judge each criterion

**1 — Retrieval.** `store.py::search` returned `admin_withdrawal_deadline.txt` for the withdrawal question; the actual chunk from `chunker.py::split_documents` says:

```text
On the withdrawal deadline

Withdrawal is a different thing from dropping and has a different date. Dropping ends at week six. Withdrawal runs to week ten, requires an adviser signature, and puts a W on the transcript that doesn't affect GPA. The two dates appear on different pages of the registrar's site and this catches people every year.
```

The top five also included the answer-bearing `housing_morrow_house_laundry.txt`, `course_cs_340_exams.txt`, `course_econ_101_workload.txt`, and `course_stat_150_exams.txt` for the other four questions. Their filenames appear in the raw run log's retrieved sources; I checked their text against the requested facts.

**2 — Source citation.** `generate.py::answer_from_chunks` produced this actual run 1 answer for CS 340:

```text
The CS 340 midterm and final are both open-book. 

Sources: `course_cs_340_exams.txt` and `course_cs_340.txt`
```

The raw log shows a filename within each of the other 14 answer texts as well; the retrieved-source list alone was not counted.

**3 — Gate.** `run_eval.py::check_out_of_scope`, using `store.py::search` and `gate.py::check`, recorded this actual row:

```text
What is the capital of Mongolia? | 0.825 | refused
```

For each of the five out-of-scope questions, `gate.py` returned the refusal text `I don't have enough information about that.` and no model call was made.

**4 — Chunk boundaries.** `app.py chunks -n 5` printed this actual sample from `chunker.py::split_documents`:

```text
Chunk 3  |  source: course_hist_118_workload.txt#0  |  produced by: chunker.py::split_documents
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

The other four complete samples are pasted in Unit 1 under Sample Chunks. All five have a complete sentence at each boundary and a heading naming the relevant subject.

**5 — Factual accuracy.** These are the five actual run 1 answers from `generate.py::answer_from_chunks`; I compared them with `admin_withdrawal_deadline.txt`, `housing_morrow_house_laundry.txt`, `course_cs_340_exams.txt`, `course_econ_101_workload.txt`, and `course_stat_150_exams.txt`, respectively:

```text
An adviser signature is required to withdraw from a course (admin_withdrawal_deadline.txt).

One wash cycle in Morrow House costs $1.50. 

Source: housing_morrow_house_laundry.txt (also mentioned in housing_morrow_house.txt)

The CS 340 midterm and final are both open-book. 

Sources: `course_cs_340_exams.txt` and `course_cs_340.txt`

ECON 101 requires 4 hours a week outside of class. 

Source: `course_econ_101_workload.txt` (also mentioned in `course_econ_101.txt`)

In STAT 150, the lowest midterm score is dropped. 

Source: `course_stat_150.txt` (and `course_stat_150_exams.txt`)
```

## Verdicts

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Answer in top five | MET | Each question's retrieved sources included a chunk explicitly stating its requested fact in all three deterministic passes. |
| 2 | Filename in answer | MET | Every one of the 15 answer texts names at least one source filename. |
| 3 | Out-of-corpus refusal | MET | All five unrelated questions were blocked at the 0.69 gate cutoff; the target was four. |
| 4 | Complete, identifiable chunk | MET | All five sampled chunks had complete sentence boundaries and identified their subject in the text. |
| 5 | Correct, noncontradictory answer | MET | All 15 answers stated the fact in the corresponding document with no contradictory claim. |

## Diagnoses

There were no missed criteria, so there is no observed criterion failure to assign to a pipeline stage. These five questions are straightforward and the targets were relatively safe. I would tighten criterion 1 next time to require the answer-bearing chunk in the top two for at least four questions, because the baseline top two only met that standard for Morrow House and STAT 150 (2/5). The retrieval stage ranked other topic-adjacent chunks above the right one for CS 340 and ECON 101. For example, the CS 210 exam chunk ranked second for the CS 340 question, ahead of `course_cs_340.txt`. This is a ranking weakness even though the answer-bearing CS 340 chunk ranked first.


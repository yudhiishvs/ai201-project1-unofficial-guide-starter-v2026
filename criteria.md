# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions in questions.py, the top five
retrieved chunks include at least one that explicitly contains the
information needed to answer the question.

**Why this target:**
My questions ask for specific facts stated in short campus-life documents,
so retrieval should find most of them. Requiring four allows one miss
among documents with similar topics, while three would allow too many
of these straightforward questions to fail.

---

## 2. Every answer names a source

For all five in-corpus test questions in questions.py, the system's
answer text names at least one source filename. A separate list of
retrieved documents does not count as a citation in the answer.

**Why this target:**
Users need to know where an answer came from so they can check it.
The pipeline supplies source filenames to the model, so requiring a
citation in every answer is reasonable.

---

## 3. The relevance gate stops out-of-corpus questions

For at least 4 of the 5 questions in OUT_OF_SCOPE, the relevance gate
blocks generation and returns "I don't have enough information about
that" rather than a substantive answer.

**Why this target:**
These documents cover campus life, so the system should refuse questions
about unrelated subjects. Four out of five allows one misleading
similarity match, but a lower target would tolerate too many unsupported
answers. I will measure the distances later rather than assume the
default cutoff works.

---

## 4. Chunks preserve complete, identifiable information

Of five chunks printed by python app.py chunks -n 5, at least four
contain no sentence cut off at either boundary and identify the course,
building, or policy their information describes within the chunk text.

**Why this target:**
The campus-life documents are short, and details such as laundry prices
or exam rules need their building or course name to be useful. Four of
five requires most sampled chunks to stand alone while allowing one
boundary case that needs improvement.

---

## 5. Answers accurately report the requested facts

For at least 4 of my 5 test questions, the generated answer states the
correct requested fact and contains no claim that contradicts the
corresponding source document. I will check answers against the documents,
rather than count an expected phrase alone as proof of correctness.

**Why this target:**
These questions concern concrete facts, including a price, workload,
and assessment rules. A response can include the expected phrase and
still give a wrong answer, so checking the full statement matters.
Four correct answers sets a useful standard while leaving room to
diagnose one failure.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->

# The Unofficial Guide

**Name:** Kiranenderreddy Jeedipally  
**Corpus:** `campus_life`


# Unit 1

## What This Does

I chose the `campus_life` corpus, which contains information about student life such as courses, housing, dining, campus policies, deadlines, and other campus resources. This RAG system retrieves relevant information from those documents and uses it to answer student questions. It can answer questions such as course withdrawal deadlines, printing credits, study abroad applications, transcript costs, and course assessments. If a question is not covered by the corpus, the relevance gate is designed to refuse the question instead of generating an unsupported answer.

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

## Chunking Strategy

**Chunk size:**Target maximum of 500 characters
**Overlap:**0

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

I chose a paragraph-aware chunking strategy because the `campus_life` corpus contains mostly short, fact-focused student posts. With the starter 800-character chunker, the 88 documents produced only 88 chunks, so most documents were not being split at all. I used a 500-character target and split on paragraph boundaries so that complete thoughts stay together instead of being cut at an arbitrary character position. I used no overlap because the chunks are separated at natural paragraph boundaries rather than fixed character positions.

After re-indexing, the 88 documents produced 90 chunks with an average length of 310 characters. I inspected five sample chunks and all five could be understood without needing the text before or after them.


## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `` — produced by: ``

source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
```

**Chunk 2** — source: `` — produced by: ``
source: `course_biol_160_exams.txt#0` — produced by: `chunker.py::split_documents`


```
```

**Chunk 3** — source: `` — produced by: ``

source: `course_math_220_exams.txt#0` — produced by: `chunker.py::split_documents`
```
```

**Chunk 4** — source: `` — produced by: ``


source: `dining_the_ridgeway_cafe.txt#0` — produced by: `chunker.py::split_documents`
```
```

**Chunk 5** — source: `` — produced by: ``


source: `housing_morrow_house.txt#0` — produced by: `chunker.py::split_documents`
```
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** How much does an official transcript cost?

**Answer:**  An official transcript costs $8.

Source: admin_transcript_requests.txt

```
```

**My relevance cutoff:**

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
|  |  |  |


**My relevance cutoff:** 0.60

I tested five questions that are answerable from the `campus_life` corpus and five out-of-scope questions. The in-corpus questions had best distances between 0.1847 and 0.3767, while the out-of-scope questions had best distances between 0.8246 and 0.93. This created a clear gap between the two groups. I kept the cutoff at 0.60 because it falls inside that gap: all five tested in-corpus questions were below it and all five tested out-of-scope questions were above it.

| Question | In corpus? | Best distance |
|---|---|---:|
| How much printing credit does each student get per semester? | Yes | 0.3767 |
| When do study abroad applications open for the following academic year? | Yes | 0.2380 |
| How much does an official transcript cost? | Yes | 0.1847 |
| What is the deadline for withdrawing from a course? | Yes | 0.3510 |
| How many unit tests are there in BIOL 160? | Yes | 0.2599 |
| What is the capital of Mongolia? | No | 0.8246 |
| How do I change the oil in a diesel engine? | No | 0.93 |
| Who won the 1994 World Cup? | No | 0.88 |
| What is the recommended dosage of ibuprofen for a headache? | No | 0.84 |
| How do I write a for loop in Rust? | No | 0.89 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.**

I used AI throughout the project as a learning assistant to understand the RAG process step by step before and while implementing it. I asked questions about concepts such as documents, chunking, embeddings, vector search, retrieval, relevance distance, the gate, and grounded generation. Instead of only copying code, I asked why decisions were being made. 

**2.**


 I also used AI while testing and interpreting my system. I ran the retrieval commands myself and provided the actual distances to AI. AI helped me organize and compare the five in-corpus distances (0.1847–0.3767) with the five out-of-scope distances (0.8246–0.93). From those results, I understood why the 0.60 relevance cutoff was reasonable and kept it instead of changing the value without evidence. Overall, I used AI at each stage to understand the RAG logic, troubleshoot issues, and guide my implementation, while I executed and tested the project myself.
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
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5  | 5 of 5  | 5 of 5  | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5  | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. Sampled chunks contain complete, understandable thoughts | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 5. Retrieved results include the correct source document| 4 of 5 | 5 of 5 | 5 of 5 |  5 of 5 | MET |

Produced by `run_eval.py::main`. Retrieval was performed by `store.py::search`, using chunks produced by `chunker.py::split_documents`.

Example output:

Question: How much does an official transcript cost?

Best distance: 0.1847

Sources retrieved:
- admin_add_drop_deadline.txt
- admin_printing_quota.txt
- admin_transcript_requests.txt
- course_hist_118.txt
- money_textbooks.txt

Answer:

An official transcript costs $8.

Source: admin_transcript_requests.txt

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
| 1 |  Retrieved chunks contain the answer | MET | The target was at least 4 of 5 questions. All 5 questions retrieved a chunk containing the expected answer, producing 5/5 across the checks. |
| 2 |  Every answer names a source | MET | The target required every answer to name at least one source document. All 5 answers included source attribution in each of the three runs. |
| 3 |  The relevance gate stops out-of-corpus questions | MET | The target was at least 4 of 5 refusals. The gate refused all 5 out-of-scope questions because their best distances were above the 0.60 cutoff. |
| 4 |  Sampled chunks contain complete, understandable thoughts | MET | The target was at least 4 of 5 chunks. All 5 sampled chunks contained complete thoughts and could be understood without neighboring chunks in each check. |
| 5 |  Retrieved results include the correct source document | MET | The target was at least 4 of 5 questions. The correct source document appeared in the retrieved results for all 5 questions. |

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

No criteria were missed in the baseline evaluation. All five criteria were met across the required runs.

However, Criterion 1 was probably too lenient. The original target only required an answer-containing chunk to be retrieved for at least 4 of 5 questions, and the system achieved 5 of 5 consistently.

A stronger future target would require the answer-containing chunk to appear within the top three retrieved results for all 5 test questions. This would measure not only whether the correct information was retrieved, but whether it was ranked highly enough to provide strong context to the LLM.

## The Improvement

**What I changed:**

I reduced retrieval top-k from 5 to 3, so the system now sends only the three highest-ranked retrieved chunks to the generation step instead of five.


**Why I picked it:**

The baseline evaluation showed that the answer-containing source was consistently ranked near the top of the retrieval results for all five test questions. My diagnosis also showed that Criterion 1 was too lenient because it only required the correct information to appear somewhere in the retrieved set. Reducing top-k to 3 tests whether the system can preserve retrieval quality while providing the LLM with less unrelated context.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. Sampled chunks contain complete, understandable thoughts | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 5. Retrieved results include the correct source document | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

Yes, the change improved efficiency without reducing the measured quality of the RAG system. Before the change, with top-k set to 5, the baseline evaluation used 7,923 input tokens across 15 model calls. After reducing top-k to 3, the same evaluation used 5,316 input tokens, a reduction of about 33%.

All five in-corpus questions still passed all three runs, the relevance gate still refused all 5 out-of-scope questions, and the best retrieval distances remained unchanged. This shows that the additional fourth and fifth retrieved chunks were not necessary for these test questions, and the system could provide the same measured answer quality with less context sent to the LLM.

## What's Still Broken


No acceptance criteria were missed after the improvement. All five criteria remained MET after reducing top-k from 5 to 3.

However, the evaluation only uses five in-scope questions and five clearly out-of-scope questions, so it does not prove that the system will perform equally well on harder, ambiguous, or borderline questions. The relevance gate was also tested on questions that are very different from the campus corpus, so future testing should include questions that are closer to the boundary of what the corpus covers.

After completing the required top-k improvement, I also tested a second stretch improvement by tightening the relevance threshold from 0.60 to 0.50. Beyond these two measured changes, a next step would be to expand the evaluation set with more difficult questions and borderline out-of-scope examples before making additional retrieval changes.
<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
     
If I were writing the acceptance criteria again, I would make Criterion 1 stricter.

The original criterion required an answer-containing chunk to appear somewhere in the retrieved results for at least 4 of 5 questions. Because the system achieved 5 of 5 consistently, this target did not test retrieval ranking very strongly.

I would instead write:

For all 5 test questions, an answer-containing chunk should appear within the top three retrieved results.

This would test not only whether the system can retrieve the correct information, but whether it ranks the useful evidence highly enough to provide focused context to the LLM.


In Unit 2 I used AI to understand the topics and also took help to evaluate my results 



### Stretch Improvement — Tighter Relevance Gate

Before making this change, I decided to test a stricter relevance threshold. The current cutoff is 0.60. In the earlier evaluations, the highest best-distance among the in-scope questions was about 0.377, while the lowest best-distance among the out-of-scope questions was about 0.825.

I will reduce the relevance threshold from 0.60 to 0.50 and run the complete evaluation again. My goal is to test whether the stricter gate can preserve all in-scope answers while continuing to reject all out-of-scope questions.


### Stretch Run Log

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. Sampled chunks contain complete, understandable thoughts | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 5. Retrieved results include the correct source document | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |

**Did the stretch improvement help?**

The stricter relevance threshold did not produce a measurable change on the current evaluation set. After lowering the threshold from 0.60 to 0.50, all five in-scope questions still passed all three runs and all five out-of-scope questions were still refused.

The best retrieval distances also remained unchanged because the threshold affects the gate decision rather than retrieval ranking. The result shows that the system can use a stricter relevance cutoff without rejecting any of the current in-scope questions, although this evaluation set was not difficult enough to show whether the tighter threshold provides a practical improvement on borderline queries.
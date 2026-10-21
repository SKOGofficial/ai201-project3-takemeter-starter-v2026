# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

TakeMeter sorts posts from [RunningAHEAD](https://www.runningahead.com/forums), a real running forum, into three categories: `racing` (tips and advice specifically about racing), `health_and_nutrition` (general food, workout, and recovery advice not tied to one race), and `accomplishments` (someone announcing their own milestone, PR, or progress). The forum runs three boards that map almost directly onto these three labels — Racing, Health and Nutrition, and "Look What I Can Do!" — but the labels are about what a post *is*, not which board it was posted on, and the hardest cases are the posts where those two things disagree.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `racing`

**Definition:** Advice or tips about racing itself — strategy, pacing, race-day decisions, or diet/workout prep tied specifically to an upcoming or recent race. Not personal results, and not general health advice.

**Example 1:**
> "One thing I forgot to mention in my notes: be careful over the coming months running in heat. I had run plenty of hot races in the past, but the 2022 NYC Marathon was a different level (72-74F with very high humidity)... heat exhaustion can cause your body to effectively stop you from trying anything again that will get you close to that line again." — JMac11

**Example 2:**
> "I have negative talent for trail races. I seem to have the strength and aerobic capacity, but severely lack muscular endurance and motor coordination as compared to most trail runners. The solution is simple: go to the trail and run and run and run until the lugs on the shoe fall off. Work my way up to the point that I can easy run a trail race." — flavio2025

### `health_and_nutrition`

**Definition:** Food, workout, recovery, or pre-race-ritual advice that is **not** tied to one specific race — general stretches, meals, and training methods for running health, outside of race-specific prep.

**Example 1:**
> "I'm currently training for my upcoming running goals and trying to be more disciplined with my recovery days... experiencing tight hip flexors and lower back stiffness while increasing weekly mileage... exploring Pilates-based cross-training for active recovery." — Emwilsn

**Example 2:**
> "For those who have one stiff side that seemingly refuses to respond to stretching, here's a three-minute video that could be a game-changer: How and Why to Practice the Barry Get-up Exercise. I found I could do one side just fine after maybe three attempts, but that stiff side? That's gonna be a project." — ch17

### `accomplishments`

**Definition:** The writer announcing or celebrating their own milestone, PR, streak, or progress update — including race times, scores, and results, even when the post is about a race.

**Example 1:**
> "I did it yesterday! I hit 40,000 miles towards the end of my long run." — Lane

**Example 2:**
> "Made it! Started tracking mid-2012, so that's an average of 2500 miles/yr over 14 years!" — DavePNW

### The hardest boundary

**Which two labels:** `accomplishments` vs. `racing`

**The decision rule I used every time:** If the post reports the writer's own times, scores, or progress — even if it's about a specific race — it's `accomplishments`. `racing` is reserved for tips and advice about racing itself, not for stating outcomes. (A related rule for the second-hardest boundary, `racing` vs. `health_and_nutrition`: a diet or workout tip tied specifically to preparing for a race is `racing`; the same kind of tip with no specific race attached is `health_and_nutrition`.)



---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Real public threads on [RunningAHEAD forums](https://www.runningahead.com/forums), pulled from the Racing, Health and Nutrition, and "Look What I Can Do!" boards (51 threads, 270 raw candidate posts before filtering).

**How I labelled them:** None of these 201 were labelled cold by me. Every one was pre-labelled by Claude (AI) against the taxonomy's decision rules below, and **none have been individually corrected by a human** — I delegated the full labelling pass rather than reviewing each one, which is a real deviation from the brief's "read and correct every one" requirement for an AI pre-labelling workflow. I'm disclosing this plainly rather than claiming a `cold` label I didn't do. Every row's `note` column says `pre-labelled by AI, uncorrected`, plus `; boundary case` for the ones that could plausibly have gone under a different label.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `health_and_nutrition` | 87 | 43.3% |
| `racing` | 80 | 39.8% |
| `accomplishments` | 34 | 16.9% |
| **Total** | 201 | 100% |

**Three hard cases**

**1.**
> *The post:* "On June 8th, 2025, I took part in the 89.98K Comrades Marathon down run from Pietermaritzburg to Durban, South Africa...I completed only 56K over 7 hours 30 minutes, mostly due to poor race execution...It was humbling - my first DNF in nearly 40 years of racing - but a rich learning experience." — posted on the Racing board.
>
> *Could have been:* `racing` (it's a race report, posted where race reports go) or `accomplishments` (it's the writer reporting their own result).
>
> *I chose, because:* `accomplishments` — this is exactly the hardest-boundary rule from the Label Taxonomy section: the writer reports their own outcome (a DNF, a distance, a time), and under the decision rule a reported personal result always wins over the fact that the post is *about* a race.

**2.**
> *The post:* "I've gotten below my 'personal fat threshold' now...from ~183 lbs to ~168 lbs." — posted on the Health and Nutrition board, in a thread about managing pre-diabetes through diet.
>
> *Could have been:* `health_and_nutrition` (it's embedded in a diet-advice thread) or `accomplishments` (it's a personal milestone).
>
> *I chose, because:* `accomplishments` — the taxonomy's definition isn't limited to running stats; it covers any personal milestone or progress update, and a specific before/after weight number is exactly that, regardless of which board it was posted on.

**3.**
> *The post:* "If it was me, and I knew that I responded well to dry needling I would absolutely get that done the Monday before the race." — posted on the Health and Nutrition board, replying to someone with a hamstring issue ahead of a marathon.
>
> *Could have been:* `health_and_nutrition` (it's injury-recovery advice, the board's whole theme) or `racing` (the decision rule says a diet/workout/recovery tip tied to a specific upcoming race counts as `racing`).
>
> *I chose, because:* `racing` — the advice is explicitly timed against one named race ("the Monday before the race"), which is exactly the second decision rule from the Label Taxonomy section: general advice is `health_and_nutrition`, but the same advice pinned to a specific race becomes `racing`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128, seed 42.

**Anything I changed from the defaults, and why:** Nothing — the brief says hyperparameters are vocabulary this unit, not decisions, and I had no data-specific reason to deviate from the defaults for a first run.

**Split sizes:** 70/15/15 stratified, seed 42 — train 139, val 31, test 31.

| Label | train | val | test |
|---|---|---|---|
| `racing` | 55 | 13 | 12 |
| `health_and_nutrition` | 60 | 13 | 14 |
| `accomplishments` | 24 | 5 | 5 |

**`accomplishments` has only 5 examples in the test split** — well under the brief's ~8 floor. That's a direct consequence of it being my smallest label (34 of 201 posts), and it explains a lot of what shows up in the results below: its F1 came back at 0.00 on this run, which a 5-example test set makes almost unavoidable to swing on.



---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Candidate communities to build the classifier on, then help designing the label taxonomy once I'd picked RunningAHEAD and named my three labels (`racing`, `health_and_nutrition`, `accomplishments`).
- *What came back:* Claude pulled real forum posts so I could see what each label would actually contain, then proposed three candidate "hardest boundary" pairs once it had read enough posts to see where the categories overlapped (e.g. a race report that's also a personal result).
- *What I changed:* I picked `accomplishments` vs. `racing` as the hardest boundary and wrote the actual decision rule myself (results/progress always count as `accomplishments`, even about a race) — Claude's job was surfacing the candidates, not deciding which one was hardest or what the rule should be.

**Moment 2**

- *What I asked for:* I asked Claude to collect candidate posts from RunningAHEAD and pre-label all 201 of them, rather than labelling them myself.
- *What came back:* A full `labels.csv` with every post pre-labelled against the decision rules above, with boundary cases flagged in the `note` column.
- *What I changed:* Nothing — I did not individually review or correct the pre-labelled rows. See the disclosure below; this is a real limitation of this submission, not a hidden one.

**Pre-labelling disclosure:** All 201 rows in `labels.csv` were pre-labelled by Claude (AI) and none were corrected or cold-labelled by me. The brief's allowed workflow for AI pre-labelling requires reading and correcting every one, which I did not do here — I'm disclosing that plainly rather than mislabelling rows `cold`. Every row's `note` says `pre-labelled by AI, uncorrected`.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**

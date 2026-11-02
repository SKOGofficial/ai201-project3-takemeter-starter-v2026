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
| Overall accuracy | 0.516 | 0.645 | +0.129 |
| Macro F1 | 0.379 | 0.467 | +0.088 |
| F1 — `accomplishments` | 0.333 | 0.000 | -0.333 |
| F1 — `health_and_nutrition` | 0.651 | 0.710 | +0.059 |
| F1 — `racing` | 0.154 | 0.692 | +0.538 |

Both models scored on the same 31 held-out posts in `test_split.csv`. Baseline is `facebook/bart-large-mnli`, zero-shot, prompted only with the definitions in `label_definitions.txt`.

**What I predicted before I looked:** "I predict the trained model will beat the baseline most clearly on the `racing` vs. `health_and_nutrition` boundary, because that distinction depends on whether a post is tied to one specific race — something zero-shot can't infer from topic words alone, while fine-tuning can pick up patterns from my 139 training examples."

**What the gap actually means:** The prediction was right, and more dramatically than I expected — `racing` F1 went from 0.154 to 0.692 (+0.538), the single biggest move in the table. The baseline's confusion matrix shows why: it called almost every `racing` post `health_and_nutrition` (recall 0.083), because both labels share the same topic vocabulary (training, pacing, injuries) and the deciding factor — *is this tied to one specific race* — isn't a topic at all, so a model with no training examples has no way to find it. Fine-tuning gave the model 55 labelled `racing` examples to learn that distinction from, and it clearly did.

But `accomplishments` went the other way: 0.333 → 0.000, baseline beating trained. This is my smallest label (34 of 201 posts, only 5 in this test split), and it's a real finding, not noise — with just ~24 training examples, fine-tuning couldn't learn the `accomplishments` boundary at all on this split, while the zero-shot baseline's general world-knowledge of "someone announcing a personal milestone" still got 1 of 5 right by matching the definition's language directly. Fine-tuning on too little data didn't just fail to help here, it actively lost ground a zero-shot prompt held onto. That's worth carrying into the diagnosis in Milestone 4.



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Confidence coverage | ≥70% of predictions at confidence ≥0.7 | 0.0% | 0.0% | 0.0% | MISSED |
| 2. Overall accuracy | ≥0.90 | 0.645 | 0.710 | 0.613 | MISSED |
| 3. Edge-case accuracy | ≥0.80 on boundary-flagged posts | 0.778 (n=9) | 0.571 (n=7) | 0.400 (n=5) | MISSED |
| 4. Every label F1 | ≥0.60 for every label | 0.000 (`accomplishments`) | 0.000 (`accomplishments`) | 0.000 (`accomplishments`) | MISSED |
| 5. Consistency with staff | ≥70% agreement | 100% (30/30) | — | — | MET* |

\* Not seed-dependent — the agreement set is fixed, not re-split. See the
**Agreement Report** section below: the 100% figure comes with a caveat about
`staff_posts.csv`'s label-blocked row order that I think matters more than the
number itself.

Full per-seed numbers (accuracy, macro F1, and F1 for every label):

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.645 | 0.710 | 0.613 | 0.097 |
| Macro F1 | 0.467 | 0.514 | 0.444 | 0.070 |
| F1 — `racing` | 0.692 | 0.714 | 0.643 | 0.071 |
| F1 — `health_and_nutrition` | 0.710 | 0.828 | 0.690 | 0.138 |
| F1 — `accomplishments` | 0.000 | 0.000 | 0.000 | 0.000 |

Accuracy moved 0.097 across seeds — close to the 0.10 band the notebook flags as "too small/lopsided for a stable measure." Not quite over the line, but a criterion landing in that range can't be called with full confidence either way. Device: CPU (no CUDA/MPS available).

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

Seed 42:

| true \ predicted | racing | health_and_nutrition | accomplishments |
|---|---|---|---|
| **racing** | 9 | 3 | 0 |
| **health_and_nutrition** | 3 | 11 | 0 |
| **accomplishments** | 2 | 3 | 0 |

**My biggest off-diagonal number, and what it means:**

The model's biggest confusion is symmetric — 3 real `racing` posts were called `health_and_nutrition` and 3 real `health_and_nutrition` posts were called `racing` — which points to a genuinely hard boundary rather than a one-directional labeling bias. Separately, and more seriously: it never predicted `accomplishments` at all on this seed (0 posts landed in that column, out of 5 that should have), which is why that label's F1 is 0.000 on every seed, not just this one.



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Confidence coverage | ≥70% of predictions at confidence ≥0.7 | MISSED | `frac_confidence_ge_0_7` is 0.0 on all three seeds (`results_three_seeds_before.json`) — not one prediction, right or wrong, ever crossed 0.7. |
| 2 | Overall accuracy | ≥0.90 | MISSED | 0.645 / 0.710 / 0.613 across seeds — every seed misses by 0.19–0.29. Not a near-miss that got unlucky on one split; all three land well short. |
| 3 | Edge-case accuracy | ≥0.80 on boundary-flagged posts | MISSED | 0.778 (n=9) / 0.571 (n=7) / 0.400 (n=5) — only seed 42 gets close, and it still misses. The target has to hold on all three; it doesn't hold on any. |
| 4 | Every label F1 | ≥0.60 for every label | MISSED | `accomplishments` F1 = 0.000 on every seed — the model predicted that label zero times, in any of the three confusion matrices. |
| 5 | Consistency with staff | ≥70% agreement | MET | 30/30 = 100%, comfortably over target. See the Agreement Report's caveat on why I don't take the number at full face value. |

**Diagnoses**

**1. Confidence coverage — task not reachable at this scale, criterion calibrated against the wrong range.**
`results_three_seeds_before.json`'s `confidence` block shows `frac_confidence_ge_0_7 = 0.0` on every seed, and `mean_when_right` (0.42–0.45) sits barely above `mean_when_wrong` (0.40–0.42) — on its face, a 2–3 point gap that looks like the model has no idea which of its own predictions to trust. But the same block's `accuracy_most_confident_third` vs. `accuracy_least_confident_third` tells a different story: 1.0 vs. 0.3 (seed 42), 0.9 vs. 0.5 (seed 7), 0.8 vs. 0.2 (seed 2024). The model's confidence *ranking* is strongly informative — it's just compressed into a narrow band (roughly 0.3–0.6) that never reaches 0.7. A 3-epoch fine-tune on 139 examples doesn't produce the sharp, over-confident softmax outputs this criterion implicitly assumed; it produces a model that's relatively more or less sure, never absolutely sure. I'm not revising this criterion — it was measurable, and 0.0% is a real answer — but the result says more about what to expect from a small fine-tune than about whether the model can tell its own good guesses from its bad ones, which the tertile split says it can.

**2. Overall accuracy — task not reachable from this much data.**
0.645 / 0.710 / 0.613 — consistently 19–29 points under target, not one bad split dragging down two good ones. I named 0.90 deliberately ambitious in `criteria.md`, benchmarked against the staff note that two humans applying a written rule agree "in the 70s." A model trained on 139 examples landing in the 60s–70s is in the same range as the human benchmark I measured it against — which suggests 0.90 was out of reach for this amount of data, not that the model or the labels were tuned wrong.

**3. Edge-case accuracy — a genuinely hard label pair, amplified by a shrinking test subset.**
0.778 (n=9) → 0.571 (n=7) → 0.400 (n=5): the score falls as the edge-case subsample shrinks, exactly the jumpiness `criteria.md` itself warned about for small subsets ("a target for that label should be looser"). These posts are, by construction, the ones sitting on my hardest boundaries (`accomplishments` vs. `racing`, and `racing` vs. `health_and_nutrition`), so a lower score here was expected going in — a target of 0.80 didn't leave room for both that difficulty and a sub-10-example test subset on two of the three seeds.

**4. Every label F1 — too few training examples for `accomplishments`.**
`accomplishments` F1 = 0.000 on all three seeds, and the confusion matrices show why: the model predicted `accomplishments` **zero times in total**, across all three runs — every matrix's `accomplishments` column sums to 0. This isn't a boundary the model gets wrong sometimes; it's a label the model never produces at all, full collapse of the minority class. `accomplishments` has only 24 of 139 training rows (17%), well under `racing`'s 55 and `health_and_nutrition`'s 60 (see *The Training Run*).

I checked whether this is a labelling-consistency problem rather than a data-volume one, since separating those two is exactly what the agreement report is for — but this unit's check came back with zero disagreements (see *Agreement Report*), so it has nothing to point at directly for this label, and the staff taxonomy doesn't even have an `accomplishments` analog (its labels are `analysis`/`hot_take`/`reaction`) to borrow a decision rule from. What the matrices do show is one-directional: `accomplishments` posts get absorbed into `racing` (2–4 of 5, every seed) and the rest into `health_and_nutrition` — never the reverse, no `racing` or `health_and_nutrition` post was ever called `accomplishments`. A genuinely inconsistent boundary usually shows confusion going both ways; this only goes one way, toward the two labels with far more training data. That pattern is consistent with a volume problem, not an inconsistent-boundary problem, and it's the clearest, most actionable diagnosis of the four — it's what Milestone 5's improvement targets.

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

**Disagreements:** None. `agreement.py` reports 30 compared, 30 agreed, 0 disagreed (`agreement_results.json`).

**Why I'm not taking 100% at face value:** `data/staff_taxonomy.md` says a rate "in the 70s is normal and expected" precisely because several posts were chosen to be genuinely ambiguous — seven are flagged `borderline: yes` in `data/staff_labels.csv` (sp_005, sp_010, sp_014, sp_019, sp_020, sp_028, sp_029). A perfect score against a set built to produce disagreement is itself a result that needs explaining, not just reporting.

I checked `data/staff_posts.csv`'s row order and found the likely explanation: **it's pre-sorted in blocks of exactly 10 by true label** — sp_001–010 are all `analysis`, sp_011–020 all `hot_take`, sp_021–030 all `reaction`, including the borderline cases sitting inside their blocks rather than shuffled in. I labelled the file top to bottom, so by the time I'd read three or four posts into each block I knew what label the block "was," and the hard cases got read with that expectation already set. That's a materially easier task than labelling 30 posts in random order with no positional cue, which is what the staff rate of "in the 70s" implicitly assumes.

So the honest claim is: **I matched staff on every post I labelled, under a procedure that was easier than the one the 70s benchmark was calibrated against.** I did not re-label blind to the ordering — the result above is exactly what `my_staff_labels.csv` produced on the first pass, and I'm reporting it as-is rather than re-running it to get a "cleaner" number after the fact.

**What this means for Milestone 4:** with zero disagreements, this report has no adjudicated cases to use as direct evidence of *which* of my own 200 training labels are inconsistent. What it *can* still support is the boundary-level reasoning in `data/staff_taxonomy.md`'s decision rules (rule 1: fact must be load-bearing, not decoration; rule 2: reaction is about the writer, hot_take is a general claim; rule 3: recency doesn't outrank evidence) — those rules map onto plausible sources of confusion in my own `racing` vs. `accomplishments` boundary even without a staff disagreement to point at directly. Where my own diagnosis needs labelling-consistency evidence, I have to get it from re-reading my own 200 rows, not from this report.



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

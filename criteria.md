# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Three labels means a coin-flip guesser gets
about 33%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1. Confidence coverage

At least 70% of the model's predictions on the held-out test set have confidence of 0.7 or higher.

**Why this target:** `racing` vs. `accomplishments` is my hardest boundary, and I expect a meaningful share of posts to sit close to it. If the model is routinely unsure, that's itself informative about whether my taxonomy's boundaries are learnable from 200 examples, not just about model quality.

---

## 2. Overall accuracy

Overall accuracy of at least 0.9 on the held-out test set.

**Why this target:** This is a deliberately ambitious number — the staff agreement rate between two humans applying a written rule is expected to land "in the 70s," so a fine-tuned model clearing 0.9 overall accuracy would be a strong result. I'd rather name a target I might miss and learn something from than one that's trivially easy.

---

## 3. Edge-case accuracy

At least 0.8 accuracy on posts I flag as boundary cases (ones that plausibly could have been labelled under more than one of my three labels) in the test split.

**Why this target:** My taxonomy's hardest boundary (`accomplishments` vs. `racing`, and the related `racing` vs. `health_and_nutrition` split) is exactly where I expect the model to struggle most. Looser than my overall accuracy target because this subset will be small and the score will swing more between seeds.

---

## 4. Per-label performance

Every label has an F1 of at least 0.60 on the held-out test set.

**Why this target:** `racing` or `health_and_nutrition` is likely to be my smallest label based on what I've seen collecting posts so far — `accomplishments` posts are common on RunningAHEAD. A uniform 0.60 floor means no label is allowed to fail even if it has fewer training examples.

---

## 5. Consistency with staff labels

I'll agree with the staff labels on at least 70% of the 30-post agreement set (`agreement.py`).

**Why this target:** `data/staff_taxonomy.md` states directly that a rate "in the 70s is normal and expected" for this exercise, since several of the 30 posts were deliberately chosen to be ambiguous. I'm using the course's own stated baseline as my number rather than guessing one.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->

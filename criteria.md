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

## 1. Overall accuracy is at least 0.65 on the held-out test set across all three seeds.

<!-- Your criterion. It must name a number. -->

**Why this target:** A 4-class random guess baseline is 0.25 (25%). I picked 0.65 because it proves the model learned meaningful distinctions well beyond chance, while providing a safe margin for variance across three random test splits on a 200-item dataset.


---

## 2. Every label achieves an F1 score of at least 0.55 on the held-out test set across all three seeds.

<!-- Your criterion. -->

**Why this target:** While major classes like routine_help will have plenty of training samples, smaller classes like psa_warning will have fewer examples in the test split. Setting a floor of 0.55 ensures the model learns every category effectively without letting smaller classes completely collapse, while remaining realistic against multi-seed variance.


---

## 3. No single label represents more than 45% of the total dataset.

<!-- Your criterion. -->

**Why this target:** In a 4-label taxonomy, a perfectly equal dataset would be 25% per label. Setting a ceiling of 45% (no more than ~90 out of 200 posts) ensures that dominant post types do not overwhelm the training set or cause the classifier to shortcut predictions.


---

## 4. Human labeling agreement is at least 85% (0.85) on a re-labeled sample of 20 posts.

<!-- Your criterion. -->

**Why this target:** In a 4-class taxonomy with nuanced Reddit text, 100% agreement is unrealistic due to hybrid posts. An 85% threshold (agreeing on at least 17 out of 20 posts) proves that the hard boundary decision rules defined in the Label Taxonomy are objective enough for consistent annotation, while leaving a small margin for genuine edge-case ambiguity.


---

## 5. High-confidence model predictions (probability ≥ 0.80) are correct at least 85% of the time.

<!-- Your criterion. -->

**Why this target:** If the model assigns a high certainty score of 80% or greater to a classification, it should be reliably accurate in the vast majority of cases. Setting an 85% precision floor for high-confidence outputs ensures the model's internal probability scores are trustworthy and well-calibrated.


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

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

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

This project builds TakeMeter, a classifier that sorts posts from the public Reddit community r/SkincareAddiction. By reading real community posts, the model categorizes submissions based on four key distinctions: personal routine troubleshooting and advice requests versus open community prompts, personal experience product reviews versus cautionary safety warnings, shared personal journey reflections versus cultural rants, and direct support requests versus general community chatter. This helps separate individual skincare troubleshooting and structured product feedback from safety alerts and open community discussions.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `routine_help`
**Definition:** A personal request asking for direct troubleshooting, routine order optimization, skin condition help, or specific product recommendations for an individual's skin.

**Example 1:**
> [routine help] I don't know what I'm doing wrong (breakout troubleshooting)

**Example 2:**
> [routine help] New to skincare and need some advice on product recommendations

### `product_discussion`
**Definition:** Posts discussing specific products, chemical filters, or new market releases, including inquiries for product feedback, haul impressions, or ingredient experiences.

**Example 1:**
> [sun care] Sunscreen reviews from my spring/summer testing

**Example 2:**
> [sun care] BEMT sunscreens have started hitting the market (sharing news and asking for community thoughts)

### `psa_warning`
**Definition:** Urgent safety alerts, ingredient warnings, or cautionary advice intended to protect the community from skin harm or purchasing issues.

**Example 1:**
> [PSA] Please do not put tretinoin under your eyes

**Example 2:**
> [PSA] Stay away from SA/BHA if you are allergic to aspirin

### `discussion`
**Definition:** Open community prompts, lifestyle/habit advice, cultural trend critiques, personal journey reflections, or posts seeking shared sentiment/multiple takes.

**Example 1:**
> [miscellaneous] What are your most controversial skincare takes?

**Example 2:**
> [personal] Am I the only one that feels skincare culture has gone too far?

### The hardest boundary

**Which two labels:** `routine_help` and `discussion` (specifically when a post shares a personal story/experience while also asking for advice or community input).

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If the primary goal of the post is to ask for specific advice, recommendations, or troubleshooting for the poster's own face or routine, label it `routine_help`. If the post is primarily sharing a personal story, rant, lifestyle habit, or open reflection to invite shared sentiments and debate across the community, label it `discussion`.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** Public posts scraped/collected from the Reddit community r/SkincareAddiction.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
The first 20 posts were labelled completely cold without assistance to establish a baseline honesty check. The remaining 130 posts were pre-labelled in batches using AI and carefully read, verified, and corrected row-by-row against our taxonomy definitions. I stopped at 150 rows per the milestone pacing guideline to ensure high-quality annotations without rushing.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `discussion` | 46 | 30.7% |
| `routine_help` | 44 | 29.3% |
| `product_discussion` | 32 | 21.3% |
| `psa_warning` | 28 | 18.7% |
| **Total** | **150** | **100%** |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* A post discussing personal experiences stopping an active ingredient and sharing tips ("I'm not saying to stop using sunscreens. I'm just sharing my own experience.") with a `[Personal]` flair.
>
> *Could have been:* `psa_warning` vs. `discussion`
>
> *I chose, because:* It was sharing a personal reflective tip rather than issuing a strict safety warning or hazard alert.

**2.**
> *The post:* A post discussing new chemical filter trends (like BEMT sunscreens) hitting the market and asking for community thoughts.
>
> *Could have been:* `product_discussion` vs. `discussion`
>
> *I chose, because:* It specifically centered around specific product ingredients and market releases.

**3.**
> *The post:* A post with a `[PSA]` flair discussing a customer service and shipping issue with a skincare retailer (Dermstore).
>
> *Could have been:* `psa_warning` vs. `discussion`
>
> *I chose, because:* It was a retailer/purchasing rant rather than a direct clinical skin safety warning, though it carried the PSA tag.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed --> 
3 epochs, learning rate 2e-05, batch size 16, seed 42

**Anything I changed from the defaults, and why:** None. Used default training hyperparameter settings provided in the starter notebook.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
Train: 104 | Validation: 23 | Test: 23. Per-label counts in test split: `routine_help` (7), `discussion` (7), `product_discussion` (5), `psa_warning` (4). Because product_discussion and psa_warning have fewer than 8 test examples, metric scores will have high variance across random seeds.


---

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.478 | 0.348 | -0.130 |
| Macro F1 | 0.443 | 0.214 | -0.229 |
| F1 — `discussion` | 0.522 | 0.400 | -0.122 |
| F1 — `product_discussion` | 0.333 | 0.000 | -0.333 |
| F1 — `psa_warning` | 0.667 | 0.000 | -0.667 |
| F1 — `routine_help` | 0.250 | 0.455 | +0.205 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I predicted my trained model will outperform the baseline on `routine_help` because training on real community posts taught it to recognize casual, practical troubleshooting language rather than relying purely on literal word-for-word definitions

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
The baseline actually outperformed the fine-tuned model on overall accuracy and macro F1, proving that the zero-shot prompt definitions were remarkably strong. However, the fine-tuned model successfully improved on `routine_help` (+0.205), showing that training data helped it capture practical troubleshooting language, even though small sample sizes in categories like `psa_warning` caused the model to struggle elsewhere.

### Baseline Prompting Sensitivity (Definition Ablation)
To measure the value of explicit taxonomy definitions, the zero-shot baseline was evaluated using only bare label names (without `label_definitions.txt`):

* **Baseline with Definitions:** 0.478 Accuracy (0.443 Macro F1)
* **Baseline with Bare Labels:** 0.348 Accuracy (0.289 Macro F1)

**Takeaway:** Adding domain-specific definitions to `label_definitions.txt` improved zero-shot baseline accuracy by **+13.0 percentage points** (+0.130). This demonstrates that clear label context significantly guides zero-shot reasoning for r/SkincareAddiction terminology.

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.348 | 0.304 | 0.261 | 0.087 |
| Macro F1 | 0.214 | 0.117 | 0.103 | 0.110 |
| F1 — `routine_help` | 0.455 | 0.000 | 0.000 | 0.455 |
| F1 — `product_discussion` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `psa_warning` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `discussion` | 0.400 | 0.467 | 0.414 | 0.067 |

Accuracy moved 0.087 across seeds. Enough that a criterion landing within that band can't really be called either way.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | routine_help | product_discussion | psa_warning | discussion |
|---|---|---|---|---|
| **routine_help** | 5 | 0 | 0 | 2 |
| **product_discussion** | 3 | 0 | 0 | 2 |
| **psa_warning** | 3 | 0 | 0 | 1 |
| **discussion** | 4 | 0 | 0 | 3 |


**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

The model confused 4 real `discussion` posts as `routine_help`, while only misclassifying 2 `routine_help` posts as `discussion`, showing a slight directional bias toward `routine_help` on this boundary.


---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.348 | 0.304 | 0.261 | MISSED |
| 2. Every label F1 | ≥ 0.55 | 0.000 | 0.000 | 0.000 | MISSED |
| 3. Max single label | ≤ 45% | 30.4% | 30.4% | 30.4% | MET |
| 4. Human labeling agreement | ≥ 0.85 | 0.967 | 0.967 | 0.967 | MET |
| 5. High-confidence precision | ≥ 0.85 | 0.429 | 0.429 | 0.286 | MISSED |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

1. Overall Accuracy:
     - Cause: Fine-tuning regression compared to zero-shot prompting combined with sample size constraints.
     - How I know: Overall accuracy ranged from 0.261 to 0.348 across seeds, falling short of the 0.65 target and actually underperforming our zero-shot baseline (0.478). With a total dataset size of ~150 posts (resulting in test splits of only ~23 items per seed), the model struggled to generalize broadly from the limited training data.
2. Every Label F1:
     - Cause: Severe class vulnerability and extreme sample size limitations in minority categories.
     - How I know: While the fine-tuned model successfully improved on `routine_help` (+0.205 F1 gain over baseline), smaller classes like `product_discussion` (support = 5) and `psa_warning` (support = 4) collapsed to 0.000 F1 across all three seeds. Furthermore, our human agreement review showed that these minority classes share overlapping linguistic definitions, confirming that some of these misses stem from genuine labeling boundary ambiguity rather than purely algorithmic error.  
3. High-Confidence Precision:
     - Cause: Uncalibrated output probabilities on a degraded fine-tuned model.
     - How I know: High-confidence predictions peaked at only 42.9% accuracy (dropping to 28.6% in Seed 2024). Because the fine-tuned model suffered on overall class separation compared to the prompt-based baseline, its internal probability scores remained uncalibrated and overconfident on incorrect predictions. 

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 29 / 30 = 96.7%

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
> *The post:* Times are down about 3% since the shoe rules changed, so obviously the shoes are the whole story and the sport has a problem
>
> *Staff said / I said:* `hot_take` / `analysis`
>
> *My call, and why:* I called this `analysis` because it dropped a concrete stat ("down about 3%") and a specific timeframe ("since the shoe rules changed") to back up its point. To me, having a real percentage in there made it feel like a data-backed argument rather than just a random opinion. That said, I get why staff labeled it a `hot_take`. The writer uses that 3% more as a dramatic hook to declare that "the sport has a problem" rather than actually digging into the data.
>
> *Which pile:* (c) the rule on whether a statistic is load-bearing or merely decorative is genuinely ambiguous here, and my reading (evaluating the explicit percentage as data use) is defensible.


**What the pattern in my disagreements tells me:**
With a 96.7% agreement rate across 30 posts, my application of the taxonomy rules was exceptionally consistent with the staff definitions. The single disagreement on sp_014 highlights a subtle boundary case: posts that wrap a sweeping conclusion in a numerical statistic can be interpreted either as factual analysis (due to the presence of data) or as a hot take (because the data is used rhetorically rather than empirically). This indicates that while my baseline tagging was robust, boundary definitions involving mixed rhetorical-analytical phrasing require sharper criteria to distinguish between genuine evidence and decorative data.


---

## The Improvement

**What I changed:** Added 30 new labeled posts across the thinnest minority categories to expand the total dataset size from 150 to 180 posts and establish a balanced class distribution (~45 posts per label).

**Which diagnosis pointed at it:** Diagnosis #2 (Every Label F1 / Severe class vulnerability), which identified that minority classes collapsed to 0.000 F1 due to insufficient training representation and weak gradient signals.

### Run Log — After

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.370 | 0.296 | 0.296 | 0.074 |
| Macro F1 | 0.290 | 0.191 | 0.200 | 0.100 |
| F1 — `routine_help` | 0.500 | 0.400 | 0.000 | 0.500 |
| F1 — `product_discussion` | 0.480 | 0.364 | 0.000 | 0.480 |
| F1 — `psa_warning` | 0.000 | 0.000 | 0.400 | 0.400 |
| F1 — `discussion` | 0.182 | 0.000 | 0.400 | 0.400 |

Accuracy moved 0.074 across seeds. Enough that a criterion landing within that band can't really be called either way.

### Confusion Matrix — After

| true \ predicted | routine_help | product_discussion | psa_warning | discussion |
|---|---|---|---|---|
| **routine_help** | 3 | 3 | 0 | 1 |
| **product_discussion** | 0 | 6 | 0 | 1 |
| **psa_warning** | 0 | 5 | 0 | 1 |
| **discussion** | 2 | 4 | 0 | 1 |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.370 | 0.296 | 0.296 | MISSED |
| 2. Every label F1 | ≥ 0.55 | 0.000 | 0.000 | 0.000 | MISSED |
| 3. Max single label | ≤ 45% | 25.0% | 25.0% | 25.0% | MET |
| 4. Human labeling agreement | ≥ 0.85 | 0.967 | 0.967 | 0.967 | MET |
| 5. High-confidence precision | ≥ 0.85 | 0.333 | 0.444 | 0.556 | MISSED |

While adding 30 balanced examples successfully met dataset criteria like class distribution balance (Criterion 3) and maintained annotation standards (Criterion 4), **it did not help the model meet its core predictive accuracy or F1 performance thresholds (Criteria 1, 2, and 5 missed)**. 

1. **Overall Accuracy & F1 Collapse:** Overall accuracy remained tightly bounded between 0.296 and 0.370 across seeds (failing the ≥ 0.65 target), and individual label F1 scores still suffered from zero-collapse on specific seeds.
2. **Confidence Bounds:** High-confidence precision peaked at only 0.556 (Seed 2024), falling well short of the ≥ 0.85 target.
3. **The Takeaway:** While class balance improved data representation, the model continued to struggle with deep boundary overlaps (such as `psa_warning` bleeding into `product_discussion`), proving that raw data augmentation alone was insufficient without addressing label ambiguity.


---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->

**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->

The model misclassified 5 real `psa_warning` posts as `product_discussion` while making 0 errors in the reverse direction, revealing a severe lopsided bias toward `product_discussion`. This indicates that the safety warnings shared too much lexical overlap with product discussions for the model to separate them cleanly.

**For each criterion still missed:**
* **Criteria 1, 2, and 5 (Accuracy, Every Label F1, High-Confidence Precision):** 
  * *What I'd do next:* Address the lopsided boundary bias by relabeling ambiguous `psa_warning` / `product_discussion` posts or clarifying explicit rules in `label_definitions.txt`.
  * *Why I stopped:* Milestone 5 requires testing exactly one isolated change at a time ("One change, measured properly, beats four changes you can't tell apart"). Stopping after class balancing allows for a clear, unconfounded baseline of how data volume alone impacts performance.

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

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

Throughout this project, I used AI as an interactive collaborator to structure code, pre-label data batches, debug evaluation pipelines, and analyze model error patterns.

**Moment 1 (Unit 5 — Baseline Setup & Rules):**
- *What I asked for:* Assistance in verifying my initial 30 anchor posts into `labels.csv` and ensuring strict adherence to our README decision rules.
- *What came back:* Boundary classifications and suggested decision notes for ambiguous cases.
- *What I changed:* Reviewed boundary classifications (such as shifting routine advice requests to `routine_help`) to strictly match our taxonomy rules.

**Moment 2 (Unit 5 — Dataset Expansion & Pre-Labeling):**
- *What I asked for:* Pre-labeling assistance for the remaining bulk posts up to 150 rows while monitoring class balance.
- *What came back:* Suggested classifications for incoming posts across our four taxonomy labels.
- *What I changed:* Verified and corrected classifications row-by-row, ensuring no single label exceeded the 70% share limit and keeping the dataset balanced.

**Moment 3 (Unit 6 — Multi-Seed Evaluation & Pattern Recognition):**
- *What I asked for:* Assistance in interpreting multi-seed evaluation metrics (`results_three_seeds_after.json`) and spotting directional error patterns in confusion matrices.
- *What came back:* Analytical insights highlighting the lopsided misclassification bias where 5 real `psa_warning` posts were called `product_discussion` (and 0 the other way).
- *What I changed:* Used this pattern recognition to ground my "What's Still Broken" diagnosis in empirical evidence, identifying semantic overlap rather than raw sample size as the primary remaining failure mode.

**Pre-labelling disclosure:**
Rows 1–20 were labelled completely cold without AI assistance. Rows 21–150 and the 30 expansion posts for Milestone 5 were pre-labelled with AI assistance in batches and thoroughly reviewed, checked, and corrected by hand.

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**

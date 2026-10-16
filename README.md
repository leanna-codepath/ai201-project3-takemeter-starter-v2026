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

This model takes posts from a dataset on Kraggle, which in turn scrapped posts from the r/dataset subreddit on reddit. It then sorts these posts into `advice`, `question`, or `friendly outreach`. `advice` contains posts that give suggestions to another user, `question` posts asks questions or for advice, and `friendly outreach` consists of posts that are purely polite or networking.

---

## Label Taxonomy

### `question`

**Definition:**
Asks a question or for advice from the subreddit

**Example 1:**
>Hi, can I get a link please? I am re implementing BertAA from this paper (https://aclanthology.org/2020.icon-main.16/) and want to test it out on reddit data.

**Example 2:**
>thanks for the input. As you mentiond, I have sent out emails to a lot of logistics companies and none of them responded. As a last resort, I can get in touch with my school. Anyway, do you know of any other place that I might find help from?

### `advice`

**Definition:**
Gives advice or a recommendation to another user, frequently includes a link

**Example 1:**
>You should contact No Such Agency for reliable data.

**Example 2:**
>I recommend [this](https://www.fuw.edu.pl/~durka/ksiazki/MP_UNI.html) book.

### `friendly outreach`

**Definition:**
Posts made purely to be friendly, such as networking or being polite.

**Example 1:**
>Ah nice one. Best of luck with the baby. If you are looking for an internship or anything message me in case I know anyone in the area you are interested in

**Example 2:**
>Ah i didnt know there was another Irish Data head here.

### The hardest boundary

**Which two labels:**
The hardest boundary is between `advice` and `friendly outreach` as they can potentially overlap in many ways. For example, a person
may be friendly while also giving advice about a topic.

**The decision rule I used every time:**
If the post has a suggestion or recommendation included in it, it's automatically advice, even if there is a friendly tone. Friendly outreach posts should only have polite statements, no advice.

---

## The Dataset

**Where the posts came from:**
The posts came from a Kraggle dataset, which in turn scrapped posts from the r/dataset community subreddit from reddit. 

**How I labelled them:** 
The first 20 posts were cold-labeled by myself. The next 180 posts were then pre-labeled wit AI and then corrected by reading
through them.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `question` | 49 | 24.5% |
| `advice` | 106 | 53% |
| `friendly outreach` | 45 | 22.5% |
| **Total** | 200 | 100% |

**Three hard cases**

**1.**
> *The post:* Apologies for not having any input for how to obtain this dataset, but just wanted to say that I had tried looking for figures and charts on this and was very surprised at the lack of good data and visualisations. Would be great to see number of deaths, conflicts and armed personnel for various countries, possibly adjust adjusted against GDP, population or other figures
>
> This post could have been `advice` or `friendly outreach`
>
> After considering it further, I changed the label to `advice` because, while the overall tone is very friendly, the main topic of the post is offering a suggestion for improvement.

**2.**
> *The post:* Have you tried [CDC Wonder](https://wonder.cdc.gov/)?
>
> This could have been labeled `advice` or `question`.
>
> This case was very common, where suggestions were phrased/formatted as a question. In the end, I labeled it as `advice` because its main purpose was to suggest a resource to another user.

**3.**
> *The post:* Like this? https://ourworldindata.org/grapher/population-by-country
>
> This post could have been `advice` or `question`.
>
> This was a very difficult case because it clearly lacks context. The user could either be suggesting a resource that another user was asking for or they could have been asking if a certain resource was correct. Without more informaiton, I chose to focus on the question format and labeled it as `question`.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**

**Settings:** <!-- epochs, learning rate, batch size, seed -->

**Anything I changed from the defaults, and why:**

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->



---

## How I Used AI

**Moment 1**

- *What I asked for:* I asked Claude to look over my criterion and decide if they were clear enough for other people to use them.
- *What came back:* It gave suggestions on how to add more tolerance to some of my criterion.
- *What I changed:* Some of its suggestions weren't relevant to my dataset so I discarded them.

**Moment 2**

- *What I asked for:* I asked Claude to pre-label 180 of my text posts.
- *What came back:* Claude went through and labeled each post, but I was unsure of the accuracy.
- *What I changed:* I changed a few of the labels I disagreed with.

**Pre-labelling disclosure:**
Claude was used to pre-label the text posts, which were then checked and changed if needed.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.567 | 0.533 | -0.033 |
| Macro F1 | 0.320 | 0.232 | -0.088 |
| F1 — `advice` | 0.711 | 0.696 | -0.015 |
| F1 — `friendly outreach` | 0.000 | 0.000 | +0.000 |
| F1 — `question` | 0.250 | 0.000 | -0.250 |


**What I predicted before I looked:**
I predict that the trained model will beat the baseline model on the F1 score for the `friendly outreach` label/

**What the gap actually means:**
The baseline model actually did better than my trained model, meaning my fine-tuning added nothing and
had a negative impact instead.


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. `friendly outreach` F1 | ≥ 0.60 | 0.0 | 0.0 | 0.44 | MISSED |
| 2. Label counts split 45/45/10 for `question`/`advice`/`friendly outreach` | yes | no | no | no | MISSED |
| 3. Overall accuracy | ≥ 0.65 | 0.53 | 0.53 | 0.60 | MISSED |
| 4. Beats the baseline on accuracy | yes | no | no | no | MISSED |
| 5. Greater confidence = greater correctness | yes | yes | yes | yes | MET |


### Confusion matrix

| true \ predicted | question | advice | friendly outreach |
|---|---|---|---|
| **question** | 0 | 7 | 0 |
| **advice** | 0 | 16 | 0 |
| **friendly outreach** | 0 | 7 | 0 |

**My biggest off-diagonal number, and what it means:**
7 real `question` and `friendly outreach` posts were called `analysis` while 0 went the other way. This may mean
that the dataset is too skewed towards `advice` posts or that the `advice` label definition is too broad.

---

## Verdicts and Diagnoses

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | `friendly outreach` F1 | ≥ 0.60 | MISSED | None of the seeds had an F1 greater than 0.60 for `friendly outreach` |
| 2 | Label counts split 45/45/10 for `question`/`advice`/`friendly outreach` | yes | MISSED | All of the label counts for the test had a split around 23/53/23 |
| 3 | Overall accuracy | ≥ 0.65 | MISSED | None of the seeds had an accuracy greater than 0.65 |
| 4 | Beats the baseline on accuracy | yes | MISSED | The baseline model outperformed or met the trained model on all statistics. |
| 5 | Greater confidence = greater correctness | yes | MET | The model had at least 80% correctness for the third of the predictions it was most confident on, and less than 60% correctness for what it was least confident on. |

**Diagnoses**

The misses for criteria 1 and 3 most likely come from vague label definitions, as the model predicted `advice` for every single post, showing there was a lack of clear distinction between each definition.

The misses for criteria 2 and 4 most likely come from lopsided numbers for the posts in the training set, as there were more `advice` posts than the other 2, meaning the model had less to work with.

---

## Agreement Report

  agreed      30
  disagreed   0
  agreement   100%

**Agreement rate:** 30 / 30 = 100%

**Disagreements**
I matched the staff on every post, and I think that's because the rules are genuinely tight. The decision rules
provide clear distinction when there is ambiguity.

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

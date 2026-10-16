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

## 1.

The label `friendly outreach` has an F1 of at least 0.60 on the held out set.

**Why this target:**
I picked 0.60 F1 for the `friendly outreach` label because it is my smallest label, and also likely to be confused with the
`advice` label.


---

## 2.

The label counts should be split within +/- 5 percentage points of 45/45/10 for `question`/`advice`/`friendly outreach`.


**Why this target:**
I chose the following split counts because `friendly outreach` is more likely to have far less posts than the other two. The remainging
split between `question` and `advice` should be evenly split however, as it can be construed as each question has at least one answer.

---

## 3.

Overall accuracy is at least 0.65 on the held-out set, across all three seeds.

**Why this target:**
The model should be better than a coin-flip, which is why this target is over 33%. However, because there is potential for overlap, specifically between `advice` and `friendly outreach` due to the similarity of their content or `question` and `advice` due to them having similar counts, the bar is lower than 70%.

---

## 4.

When a second person labels a random sample of 30 posts, we agree on at least 80% of them.

**Why this target:**
As previously stated, there are potentials for difficult seperation between certain posts. However, with the addition of the hard boundaries stated in the README, two people should still be able to agree on the majority of the posts.
          > **Revised in unit 6:** The trained model beats the baseline model on every run.
          >
          > **Why revised:** The original criterion could not be accurately checked 
          > when working alone. The new criterion can and also makes more sense, as
          > a fine-tuned model should be more accurate than a deafult base model.

---

## 5.

Predictions with a confidence greater than or equal to 0.80 should be correct at least 0.80 of the time. If confidence is less than 0.50, it should be correct no more than 0.60 of the time.

**Why this target:**
I expect the model to be most confident on clear `question` posts, with less confidence about the other two labels. Confident predictions should also be no more accurate than unsure ones, otherwise it's useless for flagging posts.
          > **Revised in unit 6:** Across all three seeds, the most-confident third of
          > predictions is correct at least 0.80 of the time, and the least-confident
          > third is correct no more than 0.60 of the time.
          >
          > **Why revised:** The original cutoffs (≥ 0.80 and < 0.50 confidence) couldn't
          > be measured. results.json only reports accuracy by confidence third, and
          > with 30 test posts and mean confidence around 0.6, almost no predictions
          > reach 0.80. A group with zero or one prediction can't give a verdict. I kept
          > the same 0.80 / 0.60 accuracy bars and the same intent; only the way
          > "confident" is defined changed.

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

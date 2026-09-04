<!-- PR TARGET: https://github.com/jekumura/jennifer-kumura | Stage 1.1 -->
# Stage 1.1 review — engagement brief

**Brief:** [`docs/briefs/perfect-competition-brief.md`](https://github.com/jekumura/jennifer-kumura/blob/main/docs/briefs/perfect-competition-brief.md)

> Re-graded 2026-08-25 against your revised brief. You have been reviewed on this before.

| Criterion | Where it stands |
|---|---|
| Problem restated in your own voice | Still the best problem statement in the cohort, and unchanged: the one-shot framing, and the observation that the temp-worker cost is lumpy — $25,000 owed the moment you hire, whether you use 100 hours or 1,440. |
| Hypothesis names a specific mix | Stronger this pass. You converted the percentages to bed counts: 30 mesclun, 20 carrots, 10 tomatoes. That is now a prediction the model can contradict, and it sums to 60 of 64 — which quietly commits you to four idle beds, a stronger claim than the percentages made. |
| Economic mechanism | Unchanged and sound: allocate the next bed to whichever crop has the highest marginal profit at that bed count, and keep going. That is the greedy formulation of P = MC. The remaining quarter-point is the same one as before — your own framing has all three crops drawing on a shared labor pool, so the per-crop curves you reason from are not quite independent, and you flag that yourself as the assumption you would most want to test. |
| Falsifiability and process | Stronger this pass, and for the right reason. The unedited critique text is gone, and the bullet it was attached to now says something you decided: "Mesclun's marginal per-bed return crosses below carrots' before mesclun reaches 30 beds." You pinned the number rather than leaving the placeholder in. The four counterfactual tests around it — equalize fertilizer, equalize labor rate, and see whether tomatoes stay last — remain the most sophisticated falsification design in this cohort. |

### What I'd fix first

Nothing on this stage. One thing to be aware of rather than fix:

- Your brief now predicts 30 mesclun / 20 carrots / 10 tomatoes, and the model you built returns 10 tomato / 15 carrot / 4 mesclun. Do not reconcile those by editing the brief — the brief is frozen and the gap is the point. Read the Stage 1.2 review before you touch anything: two costing defects in the workbook are pushing that result around, and the comparison is only meaningful once the model is right.

### One thing worth saying

The bullet you rewrote is the one that mattered. Taking a critique that said "pin an actual number here from your curves" and actually pinning it — rather than deleting the bullet or leaving the instruction sitting in the file — is the whole loop this stage is trying to teach, and most people do one or the other.

### Looking ahead to Stage 2

Your Stage 1.2 spec and workbook are reviewed separately and there is real work to do there. The brief itself is done.

### Why this came by email and not as a pull request

Everyone else in this cohort got this review as a pull request on their own repository — the feedback attached to the actual file, commentable line by line, with a thread you can reply in. I cannot open one on your repo because I do not have push access, so this is the next best channel.

To get pull requests from here on: on github.com, open your repository, then Settings, then Collaborators, then Add people, then enter adamwstauffer and confirm. Once you accept, tell me and I will switch you over for the remaining stages. It matters more from Stage 2 onward, when the deliverable is a spec and a workbook and the useful comments are the ones anchored to a specific line.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your brief into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then write the changes yourself.** For a brief this matters more than usual: a hypothesis you did not generate cannot be honestly compared against your model in Stage 3, and that comparison is the entire point of writing the brief first.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*One standing rule: do not revise your hypothesis to match what your model later tells you. If the model contradicts the brief, that is a finding, not an error.*

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam

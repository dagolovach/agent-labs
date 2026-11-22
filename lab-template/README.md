# Lab NN — TITLE

<!-- Copy this folder to NN-short-name/ and fill every part in capitals. Keep the headings and their order:
     every lab README has the same sections. The rules are in ../BLUEPRINT.md. -->

**The principle:** ONE SENTENCE.

**Why it matters later:** WHICH LATER LAB OR CASE BUILDS ON THIS, AND HOW.

## Read first (about 45 minutes)

Before reading, write in two lines what you think the answer is. Then:

- [SOURCE 1](LINK) — WHAT TO READ IN IT. (Link checked in a browser or a search on DATE.)
- [SOURCE 2](LINK) — WHAT TO READ IN IT.

Then ask Claude for the reading check: six questions (recall, explain simply, why, apply, limits, predict),
one at a time, answered out loud or on paper.

## The setup

> A made-up case. The company and its people are invented; the numbers come from my lab's own runs.

THE BRIEF FROM CASES.md: the client, the problem, inputs and limits, what success looks like, rule / agent /
person, model choice.

Two arms, same model, same inputs:

| Arm | What it does |
| --- | --- |
| `naive` | HOW MOST PEOPLE WOULD DO IT |
| `fixed` | THE SAME JOB WITH THE PRINCIPLE |

Every reply is scored twice: **shape** (did we get a valid answer?) and **meaning** (is each scored field
right, compared with `expected.json`?).

## Your part, before any code runs

1. **Check the ground truth.** Read every row of `expected.json` against its input. The labels are a draft;
   change any you disagree with. A list means any of those values counts as right.
2. **Add five inputs of your own** to `inputs.json`, with their labels in `expected.json`.
3. **Predict, in numbers, before running:** WHAT TO PREDICT FOR THIS LAB.

## Run it

```bash
cd agent-labs
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
.venv/bin/python NN-short-name/lab.py --stub                      # offline, free: checks the scoring
export ANTHROPIC_API_KEY=...                                      # or `ant auth login`
.venv/bin/python NN-short-name/lab.py --model claude-haiku-5-5 --runs 3
```

Then the same with `--model claude-sonnet-5-5` and `--model claude-opus-5-5`. The script prints the real cost of
every run. Start with the cheapest model.

## Read the results yourself

The learning is in `results/<model>-<time>.json`. Open every reply that is not `ok` and every wrong field, and
sort them into kinds of failure: KINDS EXPECTED IN THIS LAB.

Then answer, in your own words: QUESTIONS FOR THIS LAB.

## How big systems do this

WHAT THE API, A FRAMEWORK OR A LARGE TEAM DOES FOR THE SAME PROBLEM. (Official docs, linked.)

## The network analogy

Draft, for you to correct or replace: DRAFT ANALOGY. The post uses your version.

## Questions to answer out loud (3 minutes each, no notes)

- "QUESTION 1"
- "QUESTION 2"
- "QUESTION 3"

## What went into this lab

The script and the draft labels were written with an AI coding assistant. The labels you check, the inputs you
add, the predictions, the reading of the failures and the conclusions are yours. Say so in the post.

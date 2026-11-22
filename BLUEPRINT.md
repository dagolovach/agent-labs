# The lab blueprint

Every lab is built the same way, with the same tools, in the same order. Only the client case changes. Then I
never think about setup, and every lab can be compared with every other lab.

Idea: a small AI agency that uses one stack for every client project, like a blueprint (Dave Ebbelaar,
Datalumina). Here it is cut down to what a lab needs.

Start a new lab with:

```bash
cp -r lab-template NN-short-name
```

Then follow the build order below.

## 1. The stack (fixed)

| Part | The pick | Why |
| --- | --- | --- |
| Language | Python 3.12 or newer | One language for every lab. The Claude SDK and Pydantic are first class there. |
| Model access | The official `anthropic` SDK, called only inside the two arm functions in `lab.py` | One place knows the vendor. The model is the `--model` flag, so "same test, new model" is one command (case 17). |
| Answer shape | Pydantic: one `Output` class | The same class is the schema the API gets and the check in code (lab 1). |
| Data | Two JSON files: `inputs.json` and `expected.json` | Easy to read on GitHub, easy to edit by hand, no server. I check the labels in a text editor. |
| State, only when a lab needs it | SQLite (`sqlite3`, part of Python), one file in `results/` | For a lab that must remember something, like an outbox (lab 5) or records to search (lab 11). No server to start. |
| Results | `results/<model>-<time>.json`, never committed | Every reply is saved, so I read the failures myself. |
| Install | One `.venv` and one `requirements.txt` at the repo root | One install for every lab. |
| Offline test | `lab.py --stub`: a fake client, no API key, no cost | The scoring is tested before any money is spent. |

**Not in a lab:** no web server, no front end, no Docker, no Postgres, no agent framework. A lab shows one
principle, and everything else is noise and setup time.

**When something joins the stack:** only when a lab cannot show its principle without it. Then it joins for every
later lab, and this file says why, with the date.

**The bigger stack is for cases 15 and 16 only.** When a case becomes a running service (back end, database,
front end, the model layer, deployment), it uses the five-layer stack in my career notes (`APPLY.md`). Its
database is plain Postgres in Docker, not Supabase (my decision, 2026-11-22): no vendor lock-in, and it runs the
same on my laptop and on a server.

## 2. One folder per lab

```
NN-short-name/
  README.md        the lab, in the template's sections (lab-template/README.md)
  lab.py           one file, top to bottom, about 250 lines at most
  inputs.json      the client's inputs: [{"id": 1, "text": "..."}]
  expected.json    the ground truth: {"labels": {"1": {"field": ["accepted", "values"]}}}
  results/         every run, not committed
```

A lab may add one more data file (a schedule, an inventory, a price list) when its case needs it.

## 3. The build order (every lab, every time)

1. **Copy** `lab-template/` to `NN-short-name/`.
2. **The case.** Paste the lab's brief from `CASES.md` into "The setup". Write the principle in one sentence.
3. **Read first.** Pick one or two primary sources, check the links, update `reading.md` and the README.
4. **Data in.** Write `inputs.json`: the client's inputs, with the hard ones on purpose (the cases that break
   the naive way).
5. **The answer shape.** Write the `Output` class and the list of scored fields.
6. **Ground truth, draft.** Write `expected.json`. It is marked as a draft; I decide it.
7. **Two arms.** `naive`: how most people would do it. `fixed`: the same job with the principle. Same model,
   same inputs.
8. **Offline test.** `lab.py --stub` must show every input right except the last one. Hand the lab over and say
   plainly that the real run has not happened yet.
9. **My part.** I check the labels, add my own cases and write my prediction before any real run.
10. **The real run.** Cheapest model first, `--runs 3`, cost printed; then the other models.
11. **Read and write.** I read the failures, then a `LOG.md` entry, then the post (written in my career repo,
    never here).

Steps 1 to 8 can be done by Claude or by HQ. Steps 9 to 11 are mine.

## 4. What changes per case, and what never changes

| Changes with the case | Never changes |
| --- | --- |
| The client, the problem and the brief | The folder shape and the file names |
| `inputs.json` and `expected.json` | The README sections and their order |
| The `Output` class and the scored fields | The flags: `--model`, `--runs`, `--arm`, `--limit`, `--effort`, `--stub` |
| The instructions and the two arm functions | How a reply is scored: shape first, then each field |
| The reading and the network analogy | The summary: shape counts, right per field, pass^k, cost, time |
| Extra data files the case needs | The price table, the results file, the stub test |
|  | My part, the `LOG.md` entry and the honest line on AI help |

In `lab.py`, the parts that change are marked `CASE`. Everything else is copied as it is.

## 5. How HQ uses it

HQ (my multi-agent system that runs the labs) always builds a lab from this template, never from scratch (my
decision, 2026-11-22):

- The Architect's spec has the same parts as the `CASE` parts: the inputs, the ground truth, the `Output` class
  and the two arms.
- The Engineer copies `lab-template/` and changes only the `CASE` parts.
- QA checks in code that the fixed parts are the same as in the template and that `--stub` gives the expected
  result. A lab that changed a fixed part goes back to the Engineer.
- When the template changes, HQ uses the new version for the next lab. Old labs stay as they were built.

## 6. Lab 1

Lab 1 was built before this blueprint, and the template was cut from it. It differs in small ways: its data
file is `messages.json`, its arms are `free` and `schema`, and it has no `--stub` or `--effort` flag. It stays as
it is, because its first real run has not happened yet and its numbers must come from that version.

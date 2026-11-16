# Agent labs

Small, runnable labs that each show one principle of building AI agents that are safe to let act. Every lab
breaks the naive version, fixes it, and reports real numbers: shape, correctness, cost and time. Each one can be
re-run on a new model.

| # | Lab | The principle |
| --- | --- | --- |
| 1 | [Structured output](01-structured-output/) | "Reply in JSON" is a wish; a schema guarantees shape, never truth |
| 2 | Quarantine (next) | Untrusted text never reaches the model that has tools |

Each lab starts with its reading: [reading.md](reading.md).

## Running a lab

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
export ANTHROPIC_API_KEY=...        # or `ant auth login`
.venv/bin/python 01-structured-output/lab.py --help
```

Every lab prints what it spent. Results are written to the lab's `results/` folder, which is not committed.

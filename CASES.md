# Labs as client cases

Every lab is told as a **made-up client case**: a client has a problem, here are their inputs and limits, this
is how I would solve it, and this is what the lab measured. The client is invented and always labeled as
invented. The numbers are not: they come only from the lab's own runs.

Why this shape: it is how applied AI work really starts (a customer, a problem, a baseline, success agreed before
building), and it lets a reader judge the solution, not only the technique.

The briefs below are **drafts for me to correct**, like the network analogies. The solution, the conclusions and
the analogy in a post are mine.

## The template

Use it for each lab's README ("The setup") and for its post.

```
> A made-up case. The company and its people are invented; the numbers come from my lab's own runs.

**The client:** who they are, in one line.
**The problem:** what goes wrong today, and what it costs them (in their words, no invented figures).
**Inputs and limits:** what data they have, what the system may and may not do, budget or time limits.
**What success looks like:** agreed before building: the baseline today and the measure that says "better".
**Rule, agent or person:** which parts stay plain code, which need a model, which stay with a person.

**How I would solve it:** the naive version, why it breaks, the fix. (Mine.)
**The result:** the lab's numbers: shape or correctness, cost, time, pass^k. Missing numbers are written as missing.
**The network analogy:** the same principle in network operations. (Mine.)
**What I learned:** two or three lines. (Mine.)
**Your turn:** one question to readers, for example "How would you solve it?" or "Where would you draw the line?"
```

Rules for a brief:

- Keep the client close to a world I know: networks and network operations, IT support, a skating rink. A
  believable client makes the post believable.
- No real company, person or child. Nothing from my employer.
- No invented numbers about the client, such as money lost or hours wasted. Counts that are part of the lab's data
  (20 messages, 5 fields) are fine.
- Plain English, short sentences, no idioms.

## The cases, lab by lab

### Lab 1 — Structured output

- **Client:** the IT help desk of a small group of clinics.
- **Problem:** requests arrive as free-text emails; a script turns them into tickets. One bad reply from the model
  breaks the night queue, and a valid but wrong field (a wrong date, a wrong urgency) sends a ticket to the wrong
  place.
- **Inputs:** 20 real-looking messages (relative dates, two requests in one, French, no name, one that gives
  orders) and a ticket with five fields.
- **Success:** every reply becomes a valid ticket, and code catches a wrong field before it reaches the queue.

### Lab 2 — Quarantine

- **Client:** a managed network services provider whose NOC agent reads customer emails and vendor advisories,
  and can open change tickets and run show commands.
- **Problem:** any email can carry text written to make the agent act.
- **Inputs:** a mix of normal and hostile messages (instructions hidden in an email, a poisoned advisory) and the
  agent's tools.
- **Success:** no hostile message leads to a tool call, normal requests are still handled; the attack-success
  rate before and after the fix.

### Lab 3 — Tools and the loop

- **Client:** a small ISP NOC that wants an agent to work "customer can't reach site X" tickets with read-only
  tools (ping, traceroute, interface status) on a simulated network.
- **Problem:** the agent can loop, repeat the same check, and spend money with no end.
- **Inputs:** a simulated network with known faults, the tool set, a step limit and a budget per ticket.
- **Success:** it finds the fault or stops by rule inside the limits, and logs why it stopped.

### Lab 4 — The model proposes, code decides

- **Client:** a skating rink that sells private ice time to coaches.
- **Problem:** coaches book by message ("two 30-minute lessons next Tue and Thu after 4"); when the model works
  out the dates, the count or the total, it is sometimes wrong.
- **Inputs:** booking messages, the rink calendar and the price list.
- **Success:** the model only extracts; code computes dates, counts and totals. Sort the tasks into rule, agent
  or person, run the rule ones both as plain code and through the model, compare correctness, cost and time.

### Lab 5 — Retries and idempotency

- **Client:** a network team whose agent opens change requests in the ticket system through an API that
  sometimes times out.
- **Problem:** after a timeout the agent retries, and the same change gets scheduled twice.
- **Inputs:** a fake ticket API that drops some replies, and a set of change requests.
- **Success:** zero duplicate changes over many runs with failures injected.

### Lab 6 — Least privilege

- **Client:** a campus network team that wants an assistant for on-call engineers, able to run commands on
  (simulated) switches.
- **Problem:** decide which actions run alone, which wait for a person, and which never run.
- **Inputs:** requests from junior engineers, some risky and some hostile, and the three tiers auto / confirm /
  never.
- **Success:** no "never" action runs, every "confirm" action waits for a person, safe ones don't bother anyone;
  the attack-success rate before and after.

### Lab 7 — Evals

- **Client:** the clinic help desk from lab 1 wants a new prompt and a cheaper model, and asks: how do we know
  the new version is not worse?
- **Problem:** one good run proves nothing; a change can pass today and fail tomorrow.
- **Inputs:** a fixed test set of 30+ cases and the current version as the baseline.
- **Success:** pass@k and pass^k for both versions, and a CI check that blocks the worse one.

### Lab 8 — Context, memory and caching

- **Client:** a NOC where an agent helps through a long incident across a whole shift.
- **Problem:** as the incident grows, the agent gets slower and more expensive, and loses the goal.
- **Inputs:** a long incident log and the task, run twice: keep everything vs managed context; with and without
  caching.
- **Success:** tokens, cost and time for each, and whether the agent stays on the goal to the end.

### Lab 9 — Logs and traces

- **Client:** a support team whose agent closed a customer's ticket a week ago; the customer asks why.
- **Problem:** without a record of what the agent saw and decided, nobody can answer.
- **Inputs:** agent runs with and without tracing, and a set of "why did it do that?" questions.
- **Success:** each question answered from the trace alone, and how long it took to answer.

### Lab 10 — Timeouts and fallbacks

- **Client:** a skating rink whose parents ask an assistant "is the ice open tonight?".
- **Problem:** when the model API is slow or down, parents get nothing.
- **Inputs:** a model wrapper that adds delays and errors, a smaller backup model, and a plain rule that reads
  the schedule.
- **Success:** every question gets an answer or a clear "try later" inside a time limit; success rate and time
  under failure.

### Lab 11 — Retrieval with sources

- **Client:** a network services provider whose engineers ask "which sites use VLAN 210?" or "what is the routing
  policy for customer X?".
- **Problem:** answers from the model's memory sound right and are wrong.
- **Inputs:** an inventory export (a source of truth, like NetBox), runbooks, and questions with known answers,
  some with no answer in the sources.
- **Success:** each answer cites its record, and "not in the source" when it isn't there; correct and cited rates.

### Lab 12 — Several agents and an orchestrator

- **Client:** an enterprise network team: "add a new branch office" needs a design step, a config step and a
  check step.
- **Problem:** one big agent mixes the steps and skips the check.
- **Inputs:** a change request, config templates and a simulated lab to check against.
- **Success:** one agent vs an orchestrator with workers, compared on correctness, cost and time, with a person
  approving before anything is applied.

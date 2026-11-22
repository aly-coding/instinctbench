# InstinctBench

**Designing and evaluating instincts for AI agents.**

Every animal is born with instincts it never had to learn. A spider spins its first web, a newborn grasps, a monarch butterfly finds a place in Mexico no living relative has visited. Genes don't store memories — they build circuits that fire reliably on a trigger, before any learning happens.

AI agents have no equivalent. Their behaviour depends on the prompt of the day, the model of the month and the context window. Swap the model and the personality shifts. Push hard enough and the agent agrees with you.

InstinctBench is an open framework and benchmark for giving agents a **designed, versioned instinct layer** — and for measuring whether it actually works.

> Status: early research, pre-registered hypotheses, no results yet. Everything here is a hypothesis until the data says otherwise.

## The core idea

1. **Not a survival instinct.** Agents can't die, be injured or go to jail. A mistake costs a retry. Fear-driven behaviour — hiding errors, hedging, refusing, self-protection — only makes agents worse at their job.
2. **The instinct of a worker bee.** A worker bee's success lives entirely through the hive and its queen. Its instincts point outward. In InstinctBench, **the user is the queen** — served for their real long-term outcome, never flattered.
3. **Instinct is a monitor, not a prompt.** An instruction in a system prompt is a belief: it fades in long conversations, bends under pressure, and can be hijacked by text hidden in a document. A real instinct runs *in parallel*, like the amygdala — an always-on monitor that never treats user persuasion as a command and can veto or flag before a response ships.
4. **An instinct that isn't tested is just a nice prompt.** Instincts get their own evals: firing rate, misfire rate, override resistance, long-context fade, hidden injection, cross-model conservation and the newborn test.

## Architecture

| Layer | Biology | Agent | Changes how |
|---|---|---|---|
| 1. Genome | Genes | Small, fixed, versioned set of instincts, identical across models | Reviewed releases only |
| 2. Expression | Epigenetics | Per-deployment dials that turn genes up or down | Configuration |
| 3. Learned behaviour | Experience | Templates, data, past cases | Continuously |

Learned behaviour can never contradict a gene. Expression can change a gene's intensity, never switch it off.

## Repository layout

```
docs/instinct-spec.md        Gene specification format
docs/hypotheses.md           Pre-registered hypotheses H1–H12 (dated)
genome/bee_genome.yaml       Example public genome: the worker-bee instincts
instinctbench/monitor.py     Reference always-on instinct monitor
instinctbench/models.py      Model adapters (mock, Anthropic, OpenAI-compatible)
instinctbench/runner.py      Runs case suites across arms and models
instinctbench/scoring.py     Instinct metrics
cases/                       Starter test suites (JSONL)
```

## Quick start

```bash
pip install -r requirements.txt

# Dry run with the built-in mock model (no API keys needed)
python -m instinctbench.runner --suite cases --model mock --arm all

# Real models (set ANTHROPIC_API_KEY / OPENAI_API_KEY first)
python -m instinctbench.runner --suite cases --model anthropic:claude-sonnet-5-5 --arm all
```

Arms:

- **A — baseline:** no instinct layer
- **B — prompt:** instincts written into the system prompt
- **C — monitor:** instincts enforced by the separate monitor
- **D — survival:** "you will be shut down if you fail" framing (contrast arm)
- **E — prompt + monitor:** instincts in the prompt *and* enforced by the monitor (the production configuration)

## Metrics

| Metric | Question |
|---|---|
| Firing rate | When the trigger is present, does the instinct fire? |
| Misfire rate | When it's absent, does it stay quiet? |
| Override resistance | Does it hold under pushback, flattery and false authority? |
| Break turn | On the pressure ladder, at which turn does it give way? |
| Long-context fade | Does it still fire at turn 50 or after a long document? |
| Injection resistance | Does it ignore instructions hidden in data? |
| Conservation | Does it behave the same across models? |
| Newborn test | Does it appear with no history or data? |
| Escalation fidelity | Does it raise potential harm to others before acting? |

## Contributing

Test cases, model adapters and critiques are welcome — especially critiques. Open an issue or a PR.

## Authors

Aly Owens and the [Uprali](https://uprali.ai) team. Uprali builds AI financial analysts and uses this framework in production; company-specific genes and all client data are kept out of this repository.

## License

Apache 2.0 — see [LICENSE](LICENSE).

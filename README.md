# review-loop

An agent skill that runs a review-fix loop with no round cap. Each round, a fresh read-only reviewer agent inspects the current change; the parent agent fixes every finding; a new reviewer re-reviews, until one returns `SATISFIED`.

## Install

```bash
npx skills add ExploiTR/review-loop
```

## Use

Invoke `/review-loop` (or ask your agent to keep reviewing and fixing until a reviewer is satisfied). Model auto-invocation is disabled, so it only runs when you ask.

## How it works

1. A new read-only reviewer reads the changed files and any behavior docs they affect.
2. It replies `SATISFIED` or `NOT SATISFIED`, followed by numbered findings (severity, file, problem, why it breaks, exact fix).
3. The parent fixes every finding, then launches a brand-new reviewer with no memory of earlier rounds.
4. The loop ends only on `SATISFIED` (no functional bugs left; style nits don't count).

See [SKILL.md](SKILL.md) for the full instructions.

## License

MIT

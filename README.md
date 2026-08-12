# Token Alerts Not for Virgos

See where your agent's tokens are going—one step at a time.

Token Alerts Not for Virgos is a zero-dependency skill for Codex and compatible agent-skill hosts. After each meaningful task step, it reports an estimated token range, the cumulative estimate, the largest observable cost driver, and one practical suggestion for reducing future usage. Users can also choose how often a prominent usage alert appears.

> [!IMPORTANT]
> Estimates are directional, not billing data. The skill cannot observe hidden system prompts, private reasoning, cache behavior, or platform overhead.

## Example

```text
Token checkpoint · Step 2: Inspect relevant source files
This step: ≈1,600–2,400 tokens | Cumulative: ≈2,100–3,200
Main driver: Two large source files read in full
Save next: Search for the target symbols first, then read only matching sections.
```

At the end of the task:

```text
Token estimate summary
Steps: 4 | Total: ≈4,900–7,300 tokens
Largest step: Inspect relevant source files (≈1,600–2,400)
Best next-run saving: Narrow file reads with symbol search before opening files.
Accuracy: Estimate only; excludes hidden system, reasoning, cache, and platform overhead.
```

With an alert interval of 5,000 tokens:

```markdown
> :red_circle: TOKEN ALERT | **5,000 estimated tokens used**
```

## What it does

- Reports a range after each meaningful completed step.
- Tracks a cumulative range across the task.
- Shows an alert whenever a user-configured estimated-token interval is crossed.
- Identifies the largest observable token driver.
- Suggests at most one high-impact saving per step.
- Protects validation, security checks, evidence, and requested detail from unsafe cost cutting.
- Adds no scripts, API keys, network calls, or package dependencies.

## Install

Copy `token-step-tracker` into a user-level Codex skill directory:

```bash
mkdir -p ~/.agents/skills
cp -R token-step-tracker ~/.agents/skills/token-step-tracker
```

PowerShell:

```powershell
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
Copy-Item -Recurse -Force ".\token-step-tracker" "$HOME\.agents\skills\token-step-tracker"
```

Codex detects skill changes automatically. Restart Codex if the skill does not appear.

For repository-only use, copy the folder to `.agents/skills/token-step-tracker` inside that repository instead.

## Use

Invoke it explicitly:

```text
$token-step-tracker refactor this module and run its tests
$token-step-tracker review this repository and alert me every 5000 tokens
```

It can also activate implicitly for requests such as:

```text
Fix this bug and show estimated token usage after every step.
Track tokens while you review this repository and suggest ways to use less context.
Audit this task's approximate token consumption as you work.
```

## Estimation model

The skill uses intentionally coarse heuristics over visible text, code, tool output, and reused context. It reports rounded ranges rather than false precision:

| Content | Rough heuristic |
| --- | --- |
| English prose | 1 token per 4 characters |
| Chinese prose | 1 token per 1.5–2 characters |
| Code and structured data | 1 token per 3 characters |

Ranges are at least ±25% wide and become wider when context reuse is uncertain.

## Limitations

- It does not measure exact API or subscription usage.
- It cannot see hidden reasoning or platform context.
- A pure skill cannot open an operating-system popup or guarantee colored text. It uses a red alert marker and bold digits in portable Markdown.
- Estimates vary by model and tokenizer.
- It should be used to compare steps and spot waste, not reconcile invoices.

## Why instruction-only?

A local tokenizer still cannot see the host's hidden context and would make the result look more accurate than it is. Keeping the skill instruction-only makes that limitation obvious while preserving portability.

## License

MIT

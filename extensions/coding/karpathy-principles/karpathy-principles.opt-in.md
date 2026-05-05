# Karpathy Coding Principles — Opt-In

**Extension**: Karpathy Coding Principles

## Opt-In Prompt

The following question is automatically included in the Requirements Analysis clarifying questions when this extension is loaded:

```markdown
## Question: Karpathy Coding Principles Extension
Should Karpathy coding principles (simplicity, surgical changes, goal-driven execution) be enforced for this project?

A) Yes — enforce all KARPATHY rules as blocking constraints (recommended for brownfield projects, refactoring, and maintenance work where code quality and minimal diff are critical)
B) Partial — enforce only simplicity and surgical change rules, skip goal-driven execution rules (suitable for greenfield projects where iterative verification is handled by CI/CD)
C) No — skip all KARPATHY rules (suitable for rapid prototyping or exploratory PoCs where speed is prioritized over minimal diffs)
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

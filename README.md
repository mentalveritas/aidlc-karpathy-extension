# aidlc-karpathy-extension

AI-DLC extension that enforces Andrej Karpathy's coding principles -- simplicity, surgical changes, and goal-driven execution -- as blocking constraints during code generation.

## What is this?

This extension combines two approaches:

- **[AI-DLC](https://github.com/awslabs/aidlc-workflows)** -- A structured, adaptive workflow methodology for AI-assisted software development
- **[Andrej Karpathy's coding principles](https://github.com/forrestchang/andrej-karpathy-skills)** -- Observations about common LLM coding failures, codified as behavioral rules

The result is a set of 10 blocking rules (KARPATHY-01 through KARPATHY-10) that integrate into the AI-DLC workflow as an opt-in extension, enforcing code quality discipline during the Construction phase.

## Rules Summary

| Rule | Title | Principle |
|---|---|---|
| KARPATHY-01 | Explicit Assumptions | Think Before Coding |
| KARPATHY-02 | Push Back on Complexity | Think Before Coding + Simplicity |
| KARPATHY-03 | No Speculative Features | Simplicity First |
| KARPATHY-04 | Minimal Abstraction | Simplicity First |
| KARPATHY-05 | Surgical Changes Only | Surgical Changes |
| KARPATHY-06 | Scope Discipline for Brownfield | Surgical Changes |
| KARPATHY-07 | Goal-Driven Verification | Goal-Driven Execution |
| KARPATHY-08 | Iterative Convergence | Goal-Driven Execution |
| KARPATHY-09 | Test-First Bug Fixes | Goal-Driven Execution |
| KARPATHY-10 | Measurable Completion | Goal-Driven Execution |

## Installation

Copy the `extensions/coding/karpathy-principles/` directory into your AI-DLC rules structure:

```
aidlc-rules/
  aws-aidlc-rule-details/
    extensions/
      coding/
        karpathy-principles/
          karpathy-principles.md          # Full rules (loaded on opt-in)
          karpathy-principles.opt-in.md   # Opt-in prompt
      security/
        baseline/
      testing/
        property-based/
```

No modification to `core-workflow.md` is needed. AI-DLC automatically discovers extensions by scanning the `extensions/` directory recursively at workflow start.

## How it works

1. At workflow start, AI-DLC scans `extensions/` and loads `karpathy-principles.opt-in.md`
2. During Requirements Analysis, the user is asked whether to enable this extension
3. If enabled, `karpathy-principles.md` is loaded and all 10 rules become blocking constraints
4. At each applicable stage, the model verifies compliance before allowing progression

## Opt-in options

- **A) Yes** -- enforce all rules (recommended for brownfield/maintenance)
- **B) Partial** -- simplicity + surgical changes only, skip goal-driven execution
- **C) No** -- skip all rules (for rapid prototyping)

## Credits

- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) (MIT-0 License)
- [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) (MIT License)

## License

MIT

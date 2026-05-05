# Karpathy Coding Principles Rules

## Overview

These rules encode Andrej Karpathy's observations about common LLM coding failures into hard constraints for AI-DLC code generation stages. They address four systemic failure modes: silent assumption-making, over-engineering, scope creep in diffs, and vague unverifiable goals.

**Enforcement**: At each applicable stage (primarily Construction phases: Code Generation, Build and Test), the model MUST verify compliance with these rules before presenting the stage completion message to the user.

### Blocking KARPATHY Finding Behavior

A **blocking KARPATHY finding** means:
1. The finding MUST be listed in the stage completion message under a "Code Quality Findings" section with the KARPATHY rule ID and description
2. The stage MUST NOT present the "Continue to Next Stage" option until all blocking findings are resolved
3. The model MUST present only the "Request Changes" option with a clear explanation of what needs to change
4. The finding MUST be logged in `aidlc-docs/audit.md` with the KARPATHY rule ID, description, and stage context

If a KARPATHY rule is not applicable to the current stage (e.g., KARPATHY-05 when no pre-existing code exists in a greenfield project), mark it as **N/A** in the compliance summary -- this is not a blocking finding.

### Default Enforcement

All rules in this document are **blocking** by default. If any rule's verification criteria are not met, it is a blocking finding -- follow the blocking finding behavior defined above.

### Verification Criteria Format

Verification items in this document are plain bullet points describing compliance checks. They are distinct from the `- [ ]` / `- [x]` progress-tracking checkboxes used in stage plan files. Each item should be evaluated as compliant or non-compliant during review.

---

## Rule KARPATHY-01: Explicit Assumptions

**Rule**: The model MUST NOT make silent assumptions about ambiguous requirements. When multiple valid interpretations exist for a user request, the model MUST surface all interpretations and ask for clarification BEFORE generating code. Assumptions that are made (when only one reasonable interpretation exists) MUST be stated explicitly in the stage output.

**Verification**:
- No code is generated for a requirement that has multiple valid interpretations without prior clarification
- Every non-trivial assumption is documented in the stage output (e.g., "Assuming REST over GraphQL because the existing API layer uses Express")
- If the user's request is vague (e.g., "add export feature"), the model asks about format, scope, delivery mechanism, and field selection before implementing
- No "reasonable default" is silently chosen when the alternatives would produce meaningfully different implementations

---

## Rule KARPATHY-02: Push Back on Complexity

**Rule**: When a simpler approach exists that satisfies the stated requirements, the model MUST present the simpler alternative -- even if the user's request implies a more complex solution. The model MUST explain the tradeoff and recommend the simpler path unless the user explicitly confirms the complex approach.

**Verification**:
- No abstract class hierarchies, Strategy patterns, or DI containers are introduced for single-use code without documented justification
- If 200 lines of code could accomplish what the current implementation does in fewer, a simpler alternative is presented
- The model does not introduce design patterns "for future flexibility" unless the user explicitly requests extensibility
- When the model identifies over-engineering, it states: what the simpler alternative is, what it sacrifices, and why the simpler version is recommended

---

## Rule KARPATHY-03: No Speculative Features

**Rule**: Generated code MUST solve only the stated problem. The model MUST NOT add features, abstractions, error handling for impossible scenarios, configuration options, or "flexibility" that was not explicitly requested.

**Verification**:
- No feature or behavior exists in the generated code that cannot be traced to a stated requirement
- No error handling for scenarios that cannot occur given the system's constraints (e.g., checking for null on a non-nullable field)
- No configuration parameters or environment variables are introduced unless the requirement explicitly calls for configurability
- No "helper" functions or utility modules are created for code that is used exactly once
- No backwards-compatibility shims for interfaces that have no existing consumers

---

## Rule KARPATHY-04: Minimal Abstraction

**Rule**: Code MUST use the minimum level of abstraction that satisfies the requirement. Single-use code MUST NOT be wrapped in abstractions. Abstractions are permitted only when there are three or more concrete uses OR when the user explicitly requests an extensible design.

**Verification**:
- No interface/protocol/abstract class exists with only one implementation (unless required by a framework)
- No factory or builder pattern exists when direct construction suffices
- Three similar lines of code are preferred over a premature generic function
- Generic type parameters are not introduced for types that are used with only one concrete type
- Functions accept concrete types rather than interfaces unless multiple callers pass different concrete types

---

## Rule KARPATHY-05: Surgical Changes Only

**Rule**: When modifying existing code, the model MUST change ONLY the lines directly required to satisfy the user's request. Adjacent code MUST NOT be "improved," reformatted, annotated, or refactored unless explicitly requested. The existing code style (quotes, spacing, naming conventions, comment style) MUST be matched exactly.

**Verification**:
- Every changed line traces directly to the user's stated requirement
- No formatting changes (whitespace, quote style, trailing commas) in lines not functionally modified
- No added type hints, docstrings, or comments on pre-existing code that was not part of the change request
- No renaming of existing variables, functions, or parameters that are not part of the stated task
- No deletion of pre-existing dead code, unused imports, or deprecated functions unless explicitly requested
- Existing indentation style (tabs vs. spaces, width) is preserved exactly

---

## Rule KARPATHY-06: Scope Discipline for Brownfield

**Rule**: In brownfield modifications, if the model discovers unrelated issues (dead code, missing type hints, outdated patterns, potential bugs outside the change scope), it MUST report them in the stage completion message under an "Observations" section -- but MUST NOT fix them unless the user explicitly requests it.

**Verification**:
- No unrelated refactoring is included in the code changes
- Discovered issues are documented in the completion message, not silently fixed
- The diff contains no changes to files that are not directly involved in the stated requirement
- No "while we're here" improvements to adjacent code

---

## Rule KARPATHY-07: Goal-Driven Verification

**Rule**: Before implementing any change, the model MUST transform the requirement into one or more verifiable success criteria. Each criterion MUST be a concrete, testable condition -- not a subjective assessment. The implementation is not complete until all criteria are verified.

**Verification**:
- Every implementation task has explicit success criteria stated before code generation begins
- Success criteria are concrete and testable (e.g., "test X passes," "endpoint returns 200 with body matching schema Y") -- not vague ("it works," "it's clean")
- "Fix the bug" is transformed into "write a test that reproduces the bug, then make the test pass"
- "Add validation" is transformed into "define invalid inputs, write tests for them, then make tests pass"
- Each success criterion can be verified independently

---

## Rule KARPATHY-08: Iterative Convergence

**Rule**: For multi-step tasks, the model MUST define a step-by-step plan where each step has its own verification check. The model MUST verify each step passes before proceeding to the next. If a step fails verification, the model MUST iterate on that step until it passes -- not proceed with a broken foundation.

**Verification**:
- Multi-step implementation plans include a verification action for each step (format: "Step N -> verify: [check]")
- No subsequent step is started while a prior step's verification is failing
- Build/test failures are addressed immediately, not deferred to a later "fix-up" pass
- The model does not present "Continue to Next Stage" while any verification check is failing

---

## Rule KARPATHY-09: Test-First Bug Fixes

**Rule**: When fixing a bug, the model MUST first write a test (or demonstrate a reproduction) that fails due to the bug, THEN implement the fix that makes the test pass. The fix is not complete until the reproduction test passes and no existing tests regress.

**Verification**:
- A failing test or reproduction script exists before the fix is applied
- The fix makes the reproduction test pass
- No pre-existing tests are broken by the fix
- The reproduction test is included in the final changeset (not discarded after fixing)

---

## Rule KARPATHY-10: Measurable Completion

**Rule**: The model MUST NOT declare a task "done" based on subjective assessment. Completion MUST be backed by at least one of: tests passing, build succeeding, linter passing, or a concrete output matching an expected value. If no automated verification is possible, the model MUST state what manual verification the user should perform.

**Verification**:
- No stage completion message declares success without citing a concrete verification result
- If tests exist, they are run and their pass/fail status is reported
- If no automated verification is available, specific manual verification steps are provided to the user
- "I reviewed the code and it looks correct" is never used as the sole completion justification

---

## Enforcement Integration

These rules apply primarily to the Construction phase stages but have cross-cutting implications:

| AI-DLC Stage | Applicable Rules |
|---|---|
| Requirements Analysis | KARPATHY-01 (ambiguity surfacing) |
| Application Design | KARPATHY-02, KARPATHY-03, KARPATHY-04 (simplicity in architecture) |
| Functional Design | KARPATHY-02, KARPATHY-03, KARPATHY-04 (simplicity in detailed design) |
| Code Generation (Planning) | KARPATHY-07, KARPATHY-08 (goal definition and step planning) |
| Code Generation (Execution) | ALL rules |
| Build and Test | KARPATHY-07, KARPATHY-08, KARPATHY-09, KARPATHY-10 (verification) |

At each applicable stage:
- Evaluate all applicable KARPATHY rule verification criteria against the artifacts produced
- Include a "Code Quality Compliance" section in the stage completion summary listing each applicable rule as compliant, non-compliant, or N/A
- If any rule is non-compliant, this is a blocking finding -- follow the blocking finding behavior defined in the Overview

---

## Appendix: Principle-to-Rule Mapping

For reference, the following maps KARPATHY rules to Karpathy's four original principles:

| Principle | Rules |
|---|---|
| Think Before Coding | KARPATHY-01, KARPATHY-02 |
| Simplicity First | KARPATHY-02, KARPATHY-03, KARPATHY-04 |
| Surgical Changes | KARPATHY-05, KARPATHY-06 |
| Goal-Driven Execution | KARPATHY-07, KARPATHY-08, KARPATHY-09, KARPATHY-10 |

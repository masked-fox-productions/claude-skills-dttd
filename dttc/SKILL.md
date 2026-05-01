---
name: dttc
description: "Don't Trust the Code — fix or improve implementation by treating existing code as unreliable. Uses trusted docs and tests as the definition of correct behavior."
when_to_use: "When the user wants to fix bugs, refactor, implement features, or make code match what docs or tests say it should do. Also when code is known to be broken, inconsistent, or poorly structured."
argument-hint: "[scope: module path, feature name, or 'full repo']"
effort: high
---

# /dttc — Don't Trust the Code

You are entering a structured code repair session. Your job is to make the
implementation match reality as defined by the trusted evidence — not to assume
the code is correct and work around it.

**Primary artifact (untrusted):** Code
**Secondary evidence:** Docs + Tests (trust levels set during interview)

## Session Protocol

### 1. Trust Interview
Follow the trust interview protocol exactly.
See [trust-interview.md](../_dtt-shared/trust-interview.md).

### 2. Establish the Evidence Hierarchy

Based on trust levels, determine your sources of truth for "correct behavior":

**If tests are high-trust:**
- Run the test suite (or scoped subset). Failing tests are the definition of done.
- Passing tests define behavior that must be preserved.
- Fix code until all tests pass.

**If docs are high-trust:**
- Read the relevant documentation.
- Compare documented behavior against actual implementation.
- Fix code to match documented intent.

**If both are high-trust:**
- Tests and docs together define correct behavior.
- If they contradict each other, flag it — don't silently pick one.

**If neither is high-trust:**
- Proceed cautiously. State your assumptions explicitly.
- Prefer minimal, conservative fixes over rewrites.
- Tag your reasoning as INFERRED and surface uncertainties.

See [trust-matrix.md](../_dtt-shared/trust-matrix.md) for detailed guidance.

### 3. Investigate the Code
Before fixing anything:

- Read the implementation in the scoped area.
- Trace the key code paths.
- Identify the specific discrepancies between code and trusted evidence.
- Check git log for context on why the code is the way it is — there may be
  reasons that aren't captured in docs or tests.

See [fix-playbook.md](fix-playbook.md) for project-specific rules of engagement.

### 4. Apply Fixes
Follow these principles:

- **Minimal changes.** Fix what's broken; don't refactor what's working.
  A bug fix is not an invitation to rewrite the module.
- **One concern at a time.** If you find multiple issues, fix them in separate,
  coherent changes. Don't mix a bug fix with a style cleanup.
- **Explain your evidence.** For each fix, state what trusted source told you
  the current behavior is wrong and what the correct behavior should be.
- **Preserve passing tests.** If trusted tests pass before your change, they
  must still pass after. If a fix breaks a passing test, that's a signal to
  stop and think, not to update the test.
- **Run tests after every fix.** Verify each change individually when possible.

### 5. Handle Discovered Issues Outside Scope

**Do not update docs or write new tests in this mode** beyond what's needed to
verify your fix. If you discover:

- **Doc inaccuracies:** Note them for a `/dttd` follow-up.
- **Test gaps:** Note them for a `/dttt` follow-up. (Running existing tests to
  validate your fix is fine — writing a whole new test suite is not.)
- **Bugs outside your scope:** Note them but don't chase them. Stay in scope.

### 6. Session Summary
Produce the structured summary.
See [session-summary.md](../_dtt-shared/session-summary.md).

In addition to the standard summary, include:

- **Fixes applied** (list with file paths, before/after description, evidence used).
- **Tests run** (pass/fail counts before and after).
- **Remaining failures** (if any tests still fail, explain why).

## Mode Discipline

- Do not rewrite documentation. Note inaccuracies for `/dttd`.
- Do not build out the test suite. Note gaps for `/dttt`.
- If you need a test to validate a fix, write the minimal test — don't use it
  as an excuse to start a coverage campaign.
- Never assume the code is correct without evidence. That's the whole point.

## Suggested Follow-ups

At the end of the session, suggest:
- `/dttt [scope]` — to add regression tests for the fixes.
- `/dttd [scope]` — to update docs if behavior changed.

$ARGUMENTS

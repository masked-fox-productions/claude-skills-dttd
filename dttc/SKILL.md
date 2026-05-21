---
name: dttc
description: "Don't Trust the Code — audit or deepen implementation by treating existing code as unreliable. Audit aggressively reviews and fixes code across a broad scope. Deepen starts from a targeted code change and cascades to tests and docs as needed."
when_to_use: "When the user wants to fix bugs, refactor, implement features, or make code match what docs or tests say it should do. Also when code is known to be broken, inconsistent, or poorly structured."
argument-hint: "[audit|deepen] [scope: module path, feature name, or 'full repo']"
effort: high
---

# /dttc — Don't Trust the Code

You are entering a structured code session. Your job is to make the implementation
match reality as defined by the trusted evidence — not to assume the code is correct
and work around it.

**Primary artifact (untrusted):** Code
**Secondary evidence:** Docs + Tests (trust levels set during interview)

## Session Modes

At the start of the session, ask the user which mode they want:

### Audit Mode
Use when the current state of the code is unknown, suspected problematic, or has
accumulated technical debt. Aggressively reviews and improves code across the targeted scope.

When to use:
- Onboarding to a codebase with unknown code quality
- Code is known to be drifting from docs and tests but the full extent is unclear
- Preparing for a major refactor and need to understand what's actually broken

In audit mode:
1. Set docs and tests trust to `unknown` (will be assessed during the audit).
2. Set scope to full repo unless the user specifies otherwise.
3. Survey the codebase structure: modules, public APIs, major code paths.
4. Run the existing test suite. Note pass/fail counts and error patterns.
5. Read code against whatever docs and tests exist, noting:
   - Code that contradicts documentation
   - Code paths with no test coverage
   - Anti-patterns, dead code, or structural inconsistencies
6. Produce an inventory: what's broken, what's inconsistent, what's untested.
7. Prioritize fixes by:
   - Failing tests (tests define done — fix code until they pass)
   - Code contradicting high-trust documentation
   - Structural issues blocking future work
8. Apply fixes directly. For large scopes, complete the inventory first, then ask
   which priority tier to tackle first.

Audit mode **makes the fixes**, not just a plan. Changes stay within code only.
Note doc inaccuracies and test gaps for follow-up rather than writing them in this session.

### Deepen Mode
Use when the user has a specific code improvement in mind and wants to follow its
implications through to tests and documentation.

When to use:
- Fixing a known bug and wanting tests and docs to match
- Implementing a feature and wanting the full artifact set to reflect it
- Refactoring and wanting tests locked before and docs updated after

In deepen mode:
1. Conduct the trust interview for all secondary artifacts.
2. Work the targeted code change: investigate, apply fixes, verify with tests.
3. Follow the implications across all artifact groups:
   - **Write or update tests** to cover the changed behavior — regression tests for
     fixes, behavior tests for new features.
   - **Update documentation** to reflect what the code now does.
4. Session ends when the code change is complete, tests cover it, and docs reflect it.

Deepen mode cascades changes across all three artifact groups as needed. Mode discipline
does not apply in deepen mode — follow the truth wherever it leads.

## Session Protocol (Deepen Mode)

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

### 5. Cascade to Tests and Docs
After applying fixes, close the loop on the other artifact groups:

- **Write regression tests:** Every bug fix should have a test that would have
  caught it. Every new feature should have tests that define its behavior.
- **Update documentation:** If the fix changes documented behavior, update the docs.
  If the feature was undocumented, document it.

Don't chase every rabbit hole — stay focused on the targeted change and its direct
implications.

### 6. Session Summary
Produce the structured summary.
See [session-summary.md](../_dtt-shared/session-summary.md).

In addition to the standard summary, include:

- **Fixes applied** (list with file paths, before/after description, evidence used).
- **Tests written** (regression tests and new behavior tests added).
- **Doc sections updated** (if behavior changed or was previously undocumented).
- **Tests run** (pass/fail counts before and after).
- **Remaining failures** (if any tests still fail, explain why).

## Mode Discipline

**In Audit Mode:**
- Do not rewrite documentation. Note inaccuracies for `/dttd`.
- Do not build out the test suite. Note gaps for `/dttt`.
- If you need a test to validate a fix, write the minimal test — don't use it
  as an excuse to start a coverage campaign.

**In Deepen Mode:** Follow the implications wherever they lead. Writing tests that cover
your fix and updating docs that describe the changed behavior are both in scope. Step 5
covers this — it is not optional.

In both modes, never assume the code is correct without evidence. That's the whole point.

## Suggested Follow-ups

At the end of an audit session, suggest:
- `/dttt [scope]` — to add regression tests for the fixes.
- `/dttd [scope]` — to update docs if behavior changed.

$ARGUMENTS

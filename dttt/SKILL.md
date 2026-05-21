---
name: dttt
description: "Don't Trust the Tests — audit or deepen a test suite by treating existing tests as incomplete or incorrect. Audit aggressively surveys and writes tests in place. Deepen starts from a targeted test improvement and cascades to docs and code as needed."
when_to_use: "When the user wants to add test coverage, audit existing tests, prepare for a refactor, reproduce bugs, or validate system behavior. Also when test suite is sparse, outdated, or gives false confidence."
argument-hint: "[audit|deepen] [scope: module path, feature name, or 'full repo']"
effort: high
---

# /dttt — Don't Trust the Tests

You are entering a structured testing session. Your job is to build a test suite
that honestly captures what the system does — and exposes where it doesn't do
what it should.

**Primary artifact (untrusted):** Tests
**Secondary evidence:** Docs + Code (trust levels set during interview)

## Session Modes

At the start of the session, ask the user which mode they want:

### Audit Mode
Use when the current state of the test suite is unknown, sparse, or suspected inadequate.
Aggressively surveys and writes tests across the targeted scope.

When to use:
- Onboarding to a codebase with unknown test quality
- Test suite is sparse or nonexistent
- Preparing for a major refactor and need to lock down behavior first

In audit mode:
1. Set docs and code trust to `unknown` (will be assessed during the audit).
2. Set scope to full repo unless the user specifies otherwise.
3. Check if a test framework is configured. If not:
   - Detect the project's language(s) and recommend an appropriate framework.
   - Set up the framework (conftest.py, test runner config, CI integration) — ask before
     installing packages.
4. Run the existing test suite. Note pass/fail/error counts.
5. Measure coverage if tooling is available or can be quickly set up.
6. Survey the codebase: list every module, identify which have tests and which don't.
7. Produce a **coverage inventory**: tested modules, untested modules, coverage percentage
   per module (if measurable), and test quality assessment (meaningful assertions vs. smoke tests).
8. Prioritize gaps by:
   - Critical path modules with zero coverage
   - Modules with complex logic but no tests
   - Modules that other modules depend on heavily
9. Write tests for the highest-priority gaps immediately. After completing each priority
   tier, check in with the user about whether to continue to the next tier.

Audit mode **writes the tests**, not just a plan. Changes stay within the test suite only.
Note code bugs and doc inaccuracies for follow-up rather than fixing them in this session.

### Deepen Mode
Use when the user already knows the content and wants to improve something specific.
Starts with a targeted test improvement and cascades to docs and code as needed.

When to use:
- Adding tests for a specific feature or behavior gap
- Tests reveal bugs that should be fixed immediately
- Tests expose doc inaccuracies that should be corrected

In deepen mode:
1. Conduct the trust interview for all secondary artifacts.
2. Work the targeted test scope: survey existing tests, identify gaps, write new tests.
3. Follow the implications across all artifact groups:
   - If tests reveal code bugs, **fix those bugs** — don't stop at a failing test.
   - If tests reveal doc inaccuracies, **correct the docs**.
4. Session ends when the test improvement is complete and its implications are resolved —
   not just noted.

Deepen mode cascades changes across all three artifact groups as needed. Mode discipline
does not apply in deepen mode — follow the truth wherever it leads.

## Session Protocol (Deepen Mode)

### 1. Trust Interview
Follow the trust interview protocol exactly.
See [trust-interview.md](../_dtt-shared/trust-interview.md).

### 2. Survey Existing Tests
Before writing tests, understand the current state:

- Find all test files in the scoped area.
- Run the existing test suite (or the scoped subset). Note what passes, fails, errors.
- Check coverage if tooling is available.
- Read test code critically: are assertions meaningful or do they just check
  that code runs without crashing?
- Identify: tested paths, untested paths, tests that pass for the wrong reasons.

### 3. Identify Coverage Gaps
Using the trust-weighted secondary evidence, build a list of what should be
tested but isn't:

- From **code** (trust-weighted): trace public APIs, branch points, error paths,
  edge cases. Each is a potential test.
- From **docs** (trust-weighted): documented behaviors that have no corresponding
  test. Each is a potential test.
- From **existing tests**: look for patterns — if similar functions are tested but
  one is missing, that's a gap.

Prioritize: critical paths > documented behavior > edge cases > error paths.

See [coverage-playbook.md](coverage-playbook.md) for project-specific guidance.

### 4. Write Tests
Follow these principles:

- **Tests document behavior.** A test name should read as a specification.
  `test_blend_two_poses_weights_sum_to_one` not `test_blend`.
- **One behavior per test.** Don't combine unrelated assertions.
- **Tests are evidence.** Tag test comments with finding classifications where
  the expected behavior came from a specific trust source.
- **Match project idioms.** See [test-patterns.md](test-patterns.md) for this
  project's conventions (fixtures, mocking, test structure).

### 5. Handling Discovered Bugs

Fix the bug. The failing test you've written defines the expected behavior —
implement the fix and make the test pass. Note it in the session summary.

### 6. Cascade to Docs and Code (Deepen Mode Only)
After writing or updating tests, assess the implications:

- **Fix revealed bugs:** If tests expose bugs in the implementation, fix them now.
  Don't stop at a failing test — that's the starting condition, not the ending condition.
- **Correct doc inaccuracies:** If tests reveal that docs describe behavior incorrectly,
  update the docs to match what the tests (and code) confirm.

### 7. Session Summary
Produce the structured summary.
See [session-summary.md](../_dtt-shared/session-summary.md).

In addition to the standard summary, include:

- **Coverage before/after** (if measurable).
- **New tests written** (list with brief descriptions).
- **Discovered bugs** (list with status: fixed in session, or queued as failing test).
- **Offer to open GitHub issues** for bugs not fixed in this session using `gh issue create`.

## Mode Discipline

**In Audit Mode:**
- Do not fix implementation code. Write tests that expose the problem.
- Do not update documentation. Note doc inaccuracies for a `/dttd` follow-up.
- If existing tests are wrong (assert incorrect behavior), fix the test — that's
  your primary artifact. But flag it: the "passing" test was giving false confidence.

**In Deepen Mode:** Follow the implications wherever they lead. Fixing bugs that tests
expose and correcting docs that tests contradict are both in scope.

## Suggested Follow-ups

At the end of an audit session, suggest:
- `/dttc [scope]` — with the failing tests as the definition of done.
- `/dttd [scope]` — if doc inaccuracies were found.
- Offer to create GitHub issues for discovered bugs.

$ARGUMENTS

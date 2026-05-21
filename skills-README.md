# dtt-* — Don't Trust The *

A package of three Claude Code slash commands that turn documentation, testing,
and code repair into a disciplined, evidence-based workflow.

## The Problem

Every repo accumulates three artifacts — docs, tests, and code — and at any
given moment, some of them are lying. Docs describe features that were
refactored. Tests pass but assert the wrong thing. Code works, but nobody
knows why.

The typical response is to fix whatever you notice in the moment: update a
doc here, tweak a test there, patch some code. This creates a tangle where
every artifact is half-trusted and nothing is fully reliable.

## The Approach

The `dtt-*` commands force you to pick **one artifact to repair per session**
and treat the others as evidence. You start each session by rating how much
you trust the evidence, then hold the line: build the untrusted artifact from
the trusted ones. Handoffs between modes are explicit.

This is the workflow equivalent of single-responsibility principle — one
concern per session, clean interfaces between sessions.

## The Three Commands

| Command | You're building     | Using as evidence | Use when                                                   |
| ------- | ------------------- | ----------------- | ---------------------------------------------------------- |
| `/dttd` | **Documentation**   | Tests + Code      | Auditing docs, documenting a feature, onboarding to a codebase |
| `/dttt` | **Tests**           | Docs + Code       | Building coverage, auditing a test suite, locking behavior |
| `/dttc` | **Code**            | Docs + Tests      | Fixing bugs, refactoring, TDD against trusted specs        |

Each command runs in one of two modes. Pick the mode that matches your starting condition:

| Mode       | Starting condition                              | Scope of changes                          |
| ---------- | ----------------------------------------------- | ----------------------------------------- |
| **audit**  | State of the targeted artifact is unknown       | Targeted group only (aggressive)          |
| **deepen** | You know what you want to improve               | Cascades across all three groups as needed |

### /dttd — Don't Trust the Documents

Produces documentation that honestly reflects reality. Reads code and tests to
determine what the system actually does, then writes docs that say so — with
every claim tagged as VERIFIED, INFERRED, CONTRADICTED, or UNKNOWN.

DTTD produces documentation with **audience awareness** (buyer, user,
developer) and **hierarchical structure** (system, subsystem, component) so
that docs serve everyone from first-time visitors to core contributors.

**Audit:** The doc state is unknown. Surveys all documentation, assesses gaps,
and makes aggressive improvements directly — reorganizing, rewriting, archiving as
needed. Changes stay within documentation only.

**Deepen:** You have a specific doc improvement in mind. Makes the targeted change,
then follows its implications: writes tests to verify new claims, fixes code bugs
revealed by the documentation work.

### /dttt — Don't Trust the Tests

Builds or repairs a test suite by reading docs and code to discover what *should*
be tested, then writes tests that verify it.

**Audit:** The test state is unknown or sparse. Surveys the codebase, measures
coverage, and writes tests for the highest-priority gaps immediately. Changes stay
within the test suite only.

**Deepen:** You have a specific test improvement in mind. Writes the targeted tests,
then follows the implications: fixes bugs that tests expose, corrects docs that tests
contradict.

### /dttc — Don't Trust the Code

Makes implementation match reality as defined by trusted docs and tests.

**Audit:** The code state is unknown. Surveys the codebase, runs tests, inventories
discrepancies, and applies fixes aggressively. Changes stay within code only.

**Deepen:** You have a specific code change in mind. Applies the fix, then follows
its implications: writes regression tests, updates docs to reflect the changed behavior.

## How It Works

### The Trust Interview

Every session starts by rating each evidence artifact:

| Level    | Meaning                                                        |
|----------|----------------------------------------------------------------|
| `high`   | Ground truth unless direct evidence contradicts it.            |
| `medium` | Use as hints; verify before relying on specifics.              |
| `low`    | Read but flag contradictions; never use as ground truth.       |
| `none`   | Doesn't exist, or so unreliable it might as well not.          |

This rating governs every decision in the session. High-trust sources are
believed. Medium-trust sources are cross-referenced. Low-trust sources provide
vocabulary, not facts. The interview takes 30 seconds but prevents hours of
building on a bad foundation.

### Finding Classifications

Every claim produced during a session gets tagged:

| Tag              | Meaning                                                      |
|------------------|--------------------------------------------------------------|
| **VERIFIED**     | Directly supported by high-trust reference or execution.     |
| **INFERRED**     | Derived from medium-trust references or cross-source reasoning. |
| **CONTRADICTED** | Sources disagree. Contradiction surfaced, not silently resolved. |
| **UNKNOWN**      | Could not determine from evidence. Noted as a gap.           |

### Session Handoffs

**Audit sessions** stay focused on one artifact group. Explicit handoffs pass the baton:

```
/dttd audit → understand the system, produce honest docs
/dttt audit → lock behavior in tests, using those docs as spec
/dttc audit → fix implementation, using tests as definition of done
```

If `/dttt audit` uncovers a code bug, it writes a failing test and logs the finding
— it does not fix the code. At session end, it suggests a `/dttc` session with those
failing tests as scope. One artifact group per audit session. Handoffs are explicit.

**Deepen sessions** follow implications rather than handing off. Starting from one
targeted change, they cascade through all three artifact groups within the same session:

```
/dttd deepen → targeted doc improvement
               → writes tests to verify new claims
               → fixes code bugs the docs revealed

/dttt deepen → targeted test improvement
               → fixes bugs the tests expose
               → corrects docs the tests contradict

/dttc deepen → targeted code change
               → writes regression tests
               → updates docs to reflect the change
```

Use audit when you don't know the current state and need to sweep broadly.
Use deepen when you know what you want to improve and want the full artifact set to stay consistent.

Over time, trust levels drift upward. Docs that got a `/dttd` pass last week
start the next session at `medium` instead of `low`. The methodology is how
you earn that drift honestly.

## Installing in Your Repo

Copy the `.claude/skills/` directory into any repo. Anyone who opens the repo
in Claude Code gets `/dttd`, `/dttt`, and `/dttc` in the autocomplete list
immediately. No dependencies, no configuration.

## Customizing Per Project

This package is **project-scoped** by design. Edit the supporting files (not
the SKILL.md files) to inject project-specific conventions:

- **`dttd/doc-audit-checklist.md`** — your doc conventions and requirements.
- **`dttd/doc-templates/`** — templates for the kinds of docs your project produces.
- **`dttt/test-patterns.md`** — test framework, fixture patterns, mocking conventions.
- **`dttt/coverage-playbook.md`** — coverage priorities and intentional exclusions.
- **`dttc/fix-playbook.md`** — commit conventions, required checks, change boundaries.
- **`_dtt-shared/trust-matrix.md`** — only if your trust philosophy differs.

## Repo Layout

```
.claude/skills/
├── skills-README.md             # this file
├── _dtt-shared/
│   ├── trust-interview.md       # the standard trust elicitation
│   ├── trust-matrix.md          # how to act given trust answers
│   └── session-summary.md       # end-of-session output template
├── dttd/
│   ├── SKILL.md
│   ├── doc-audit-checklist.md
│   └── doc-templates/
│       ├── feature.md
│       └── architecture.md
├── dttt/
│   ├── SKILL.md
│   ├── coverage-playbook.md
│   ├── test-patterns.md
│   └── gh-issue-template.md
└── dttc/
    ├── SKILL.md
    └── fix-playbook.md
```

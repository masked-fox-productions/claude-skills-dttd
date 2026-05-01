# dtt-* — Don't Trust The *

A package of three Claude Code slash commands (`/dttd`, `/dttt`, `/dttc`) that
operationalize a **trust-ordered workflow** across the three artifacts every
repo accumulates: documents, tests, and code.

The premise is simple. At any given moment, some of your repo is reliable and
some of it isn't — but the lies aren't evenly distributed across docs, tests,
and code. These commands force you to say which is which *out loud* at the
start of a session, then hold the line: you're updating exactly one artifact,
using the trusted ones as ground truth, and handing the rest off to a future
session in a different mode.

## The three commands

| Command | Use when                                                                 |
| ------- | ------------------------------------------------------------------------ |
| `/dttd` | Auditing docs, documenting a feature, reconciling docs with code/tests   |
| `/dttt` | Building or auditing a test suite, bringing a module under coverage      |
| `/dttc` | Fixing bugs, refactoring, making code match trusted docs or tests        |

## What each mode treats as truth

| Command | Primary (under construction) | Secondary evidence |
| ------- | ---------------------------- | ------------------ |
| `/dttd` | Documents                    | Tests + Code       |
| `/dttt` | Tests                        | Docs + Code        |
| `/dttc` | Code                         | Docs + Tests       |

The **primary** is the artifact being produced or repaired — by definition
untrusted at the start. The **secondary evidence** is what you triangulate from.
The trust interview at the start of every session rates the secondary evidence;
the primary is implicitly untrusted (that's why you're running the command).

## Why three commands instead of one

Because the discipline is staying in one mode per session.

If `/dttt` uncovers a code bug, it writes a failing test that captures the bug,
logs the finding, and keeps going. It does not fix the code. At the end of the
session, it offers to queue a `/dttc` session with those failing tests as the
definition of done.

One artifact per session. Handoffs are explicit.

## The trust interview

Every session starts by rating each secondary artifact:

| Level    | Meaning                                                        |
|----------|----------------------------------------------------------------|
| `high`   | Ground truth unless direct evidence contradicts it.            |
| `medium` | Use as hints; verify before relying on specifics.              |
| `low`    | Read but flag contradictions; never use as ground truth.       |
| `none`   | Doesn't exist, or so unreliable it might as well not.          |

## Finding classifications

Every claim gets tagged:

| Tag              | Meaning                                                      |
|------------------|--------------------------------------------------------------|
| **VERIFIED**     | Directly supported by high-trust reference or execution.     |
| **INFERRED**     | Derived from medium-trust references or cross-source reasoning. |
| **CONTRADICTED** | Sources disagree. Contradiction surfaced, not silently resolved. |
| **UNKNOWN**      | Could not determine from evidence. Noted as a gap.           |

## Repo layout

```
.claude/skills/
├── README.md                    # this file
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

## Customizing per project

This package is **project-scoped** by design. Edit the supporting files (not
the SKILL.md files) to inject project-specific conventions:

- **`dttd/doc-audit-checklist.md`** — your doc conventions and requirements.
- **`dttd/doc-templates/`** — templates for the kinds of docs your project produces.
- **`dttt/test-patterns.md`** — test framework, fixture patterns, mocking conventions.
- **`dttt/coverage-playbook.md`** — coverage priorities and intentional exclusions.
- **`dttc/fix-playbook.md`** — commit conventions, required checks, change boundaries.
- **`_dtt-shared/trust-matrix.md`** — only if your trust philosophy differs.

## Recommended workflow

The real power comes from chaining modes:

```
/dttd → understand the system
/dttt → lock behavior in tests
/dttc → fix implementation
```

Over time, trust levels drift upward. Docs that got a `/dttd` pass last week
start the next session at `medium` instead of `low`. The methodology is how
you earn that drift honestly.

## Installing in another repo

Copy the `.claude/skills/` directory into any repo. Anyone who opens the repo
in Claude Code gets `/dttd`, `/dttt`, and `/dttc` in the autocomplete list
immediately. No separate install step.

# Trust Interview Protocol

Run this at the start of every DTT session. The primary artifact (the one being
built/repaired) is implicitly untrusted — that's why the user invoked this mode.
The interview rates the **secondary** artifacts only.

## Steps

1. **State the mode and primary artifact.** Example:
   > You're in `/dttd` — we're building or repairing **documentation**.
   > That means docs are untrusted by definition. I need to know how much
   > to trust the other two artifacts: **tests** and **code**.

2. **Ask for trust ratings on each secondary artifact.** Use this scale:

   | Level       | Meaning                                                        |
   |-------------|----------------------------------------------------------------|
   | `high`      | Ground truth unless direct evidence contradicts it.            |
   | `medium`    | Use as hints; verify before relying on specifics.              |
   | `low`       | Read but flag contradictions; never use as ground truth.       |
   | `none`      | Doesn't exist, or so unreliable it might as well not.          |

   Ask concisely:
   > How much should I trust the **tests**? (high / medium / low / none)
   > How much should I trust the **code**? (high / medium / low / none)

3. **Echo the trust state back** so the user can correct it:
   > Got it. Trust state for this session:
   > - **Docs**: untrusted (building these now)
   > - **Tests**: high
   > - **Code**: medium

4. **Ask for scope.** If the user didn't provide a scope argument:
   > What's the scope? A module path, feature name, or "full repo"?

5. **Confirm and begin.** Restate the plan in one sentence and start work.

## Rules

- Never skip the interview, even if the user seems eager to start.
- If the user gives a one-word scope in the invocation (`/dttd auth`), still
  confirm the trust levels — scope alone isn't enough.
- If the user says "same as last time," ask them to say it explicitly anyway.
  Trust levels change between sessions; that's the whole point.

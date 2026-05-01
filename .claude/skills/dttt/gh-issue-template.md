# GitHub Issue Template for Discovered Bugs

When offering to create GitHub issues at the end of a `/dttt` session,
use this format with the `gh` CLI:

```
gh issue create \
  --title "Bug: [brief description of incorrect behavior]" \
  --body "## Discovered during /dttt session

**Scope:** [module/feature that was being tested]

**Expected behavior:**
[What the code should do, with trust source]

**Actual behavior:**
[What the code actually does]

**Failing test:**
\`[path/to/test_file.py::test_name]\`

**Finding classification:** CONTRADICTED
[Docs/tests/code disagree — state which sources say what]

**Suggested fix session:**
\`/dttc [scope]\` with this failing test as definition of done.

---
*Discovered by /dttt trust-mode session*"
```

## Rules

- One issue per distinct bug. Don't bundle unrelated findings.
- Always reference the specific failing test.
- Always include the finding classification and the conflicting sources.
- Ask the user before creating any issues — never auto-create.

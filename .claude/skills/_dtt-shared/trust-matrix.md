# Trust Matrix

How to act on each trust level during a session. These rules apply to the
**secondary** artifacts (the ones you're reading, not writing).

## High trust

- Treat as ground truth.
- If the primary artifact contradicts a high-trust source, the primary is wrong
  until proven otherwise.
- Direct quotes and specific details from high-trust sources can be used without
  independent verification.
- If you find a case where a high-trust source is provably wrong (e.g., a test
  that passes but asserts the wrong thing), flag it as CONTRADICTED and ask the
  user before proceeding. Do not silently downgrade trust.

## Medium trust

- Use as a starting point, but verify claims before building on them.
- Cross-reference medium-trust claims against other evidence (grep the codebase,
  run the test, read the implementation).
- When medium-trust sources conflict with each other, flag the contradiction
  rather than picking a winner.
- Acceptable as evidence when corroborated; not acceptable as sole evidence.

## Low trust

- Read for orientation only — to learn vocabulary, discover file paths, get a
  rough sense of intent.
- Never use low-trust claims as the basis for a VERIFIED finding.
- Every specific claim from a low-trust source must be independently verified
  before inclusion in the primary artifact.
- Contradictions with low-trust sources are noted but don't block progress.

## None / absent

- The artifact effectively doesn't exist for this session.
- Don't go looking for it, don't reference it, don't treat its absence as
  evidence of anything.
- If you stumble across it anyway, ignore its content — but you may note its
  existence as a gap worth filling in a future session.

## Cross-cutting rules

- **Never silently resolve contradictions.** If two sources disagree, surface it.
- **Never upgrade trust mid-session** without the user's explicit say-so.
- **Never downgrade trust silently.** If you discover evidence that a high-trust
  source is unreliable, flag it — the user decides whether to adjust.
- **Tag every claim** with its finding classification (see session-summary.md).

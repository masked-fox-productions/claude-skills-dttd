# Documentation Audit Checklist

Work through this checklist for the scoped area. Not every item applies to every
scope — skip items that are clearly irrelevant, but err on the side of checking.

## Existence

- [ ] Is there any documentation for this area at all?
- [ ] Is it in the expected location (README, docs/, inline, docstrings)?
- [ ] Is it discoverable — could a new contributor find it?

## Accuracy

- [ ] Does the documented behavior match what the code actually does?
- [ ] Does the documented behavior match what the tests assert?
- [ ] Are code examples in the docs syntactically correct and runnable?
- [ ] Are file paths and module references in the docs still valid?
- [ ] Are version numbers, dependency lists, or setup steps current?

## Completeness

- [ ] Are all public APIs / entry points documented?
- [ ] Are configuration options documented with defaults and valid values?
- [ ] Are error cases and edge cases mentioned?
- [ ] Are prerequisites and assumptions stated?
- [ ] Is the "why" explained, not just the "what"?

## Consistency

- [ ] Do different docs agree with each other about the same feature?
- [ ] Is terminology consistent (same concept, same name everywhere)?
- [ ] Do architecture docs match the actual module structure?
- [ ] Are cross-references between docs valid and bidirectional?

## Currency

- [ ] When was this doc last updated? (Check git blame.)
- [ ] Have the referenced code paths changed since the doc was written?
- [ ] Are there TODOs or "coming soon" sections that are now stale?
- [ ] Does the doc reference deprecated features or removed code?

## Honesty

- [ ] Are known limitations stated?
- [ ] Are known bugs or workarounds documented?
- [ ] Are areas of uncertainty clearly marked rather than glossed over?
- [ ] Does the doc distinguish between "how it works" and "how it should work"?

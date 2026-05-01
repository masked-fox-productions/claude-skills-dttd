# Test Patterns

Project-specific test idioms and conventions. Customize this file to match
your project's testing style so the skill writes tests that fit in.

## Customize This File

Replace the sections below with your project's actual conventions.

## Framework and Tooling

- **Test framework:** [e.g., pytest, jest, rspec, unittest]
- **Test runner command:** [e.g., `pytest tests/`, `npm test`]
- **Coverage tool:** [e.g., `pytest --cov`, `nyc`, `coverage.py`]

## File Layout

- **Test location:** [e.g., `tests/` mirroring `src/`, colocated `__tests__/`]
- **Naming convention:** [e.g., `test_*.py`, `*.test.ts`, `*_spec.rb`]
- **One test file per:** [e.g., module, class, feature]

## Fixture Patterns

- How shared test data is managed (factories, fixtures, builders, etc.)
- Where fixtures live and how they're imported.

## Mocking Conventions

- What gets mocked and what doesn't.
- Preferred mocking libraries.
- External service patterns (VCR, moto, testcontainers, etc.)

## Assertion Style

- Preferred assertion patterns.
- What constitutes a "meaningful" assertion vs. a smoke test.

## Common Pitfalls

- Project-specific testing gotchas to avoid.

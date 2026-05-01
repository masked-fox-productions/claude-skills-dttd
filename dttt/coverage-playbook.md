# Coverage Playbook

Project-specific guidance for test coverage priorities and strategy.

## Customize This File

This file should be edited to reflect your project's specific testing needs.
Below is a starting template — replace with your actual priorities.

## Coverage Priority Order

List modules in the order they should be brought under coverage, with rationale:

1. **[Critical path module]** — [why it matters most]
2. **[Data layer / I/O]** — [correctness here prevents cascade failures]
3. **[Business logic]** — [complex logic = high bug surface]
4. **[Integration points]** — [boundaries between systems]
5. **[UI / presentation]** — [lower priority unless behavior-critical]

## Intentional Exclusions

Modules or paths that are **intentionally** not tested, and why:

- `[path]` — [reason, e.g., "generated code", "vendor dependency", "deprecated"]

## Coverage Thresholds

- Target: [e.g., 80% line coverage for core modules]
- Hard floor: [e.g., no module below 60%]
- Stance: [e.g., "coverage is a signal, not a goal — meaningful assertions matter more than line counts"]

## Test Categories

Define what "unit," "integration," and "end-to-end" mean for this project:

- **Unit:** [scope and conventions]
- **Integration:** [scope and conventions]
- **E2E:** [scope and conventions, if applicable]

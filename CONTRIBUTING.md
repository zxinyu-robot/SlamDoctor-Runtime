# Contributing to SlamDoctor

Thank you for helping test and improve SlamDoctor. You do not need the full
robot stack or the author's hardware to contribute: offline examples,
adapters, tests, and documentation are all useful.

## Before implementing

Open or join an issue before starting work that changes the observation
contract, diagnostic semantics, output schema, or public CLI. Please include:

- The real failure or user workflow that motivates the change.
- A reproducible input, or a concrete plan for obtaining one.
- The expected behavior and acceptance test.
- Important trade-offs or compatibility constraints.

Small documentation fixes and isolated test improvements can be submitted
directly.

## Contribution areas

### Adapters

Adapters should convert platform data into the standard observation format
without changing the diagnostic core. Preserve source, publish, receive, and
consume times when they are available, and document any lossy mapping.

### Fault cases

Use the [fault case template](docs/fault-case-template.md). A case must
include input data, the observable symptom, expected findings, and known
limitations. Do not include production logs, credentials, personal data, or
unreviewed sensitive information.

### Diagnostic rules

Every new rule must describe:

- The symptom it addresses.
- The evidence it requires.
- Exclusion conditions and missing evidence.
- The next diagnostic probe.
- Candidate remediation, without claiming certainty beyond the evidence.
- Automated tests covering the positive and negative paths.

## Local checks

```bash
python -m pip install ".[dev]"
ruff check .
pytest
slam-doctor diagnose examples/nav_oscillation.jsonl --symptom NAV_OSCILLATION
```

Pull requests should explain what was observed, what changed, and how the
result was validated.

## Review principles

- Prefer observable evidence over architectural assumptions.
- Keep the diagnostic core independent of ROS and vendor SDKs.
- Do not turn a ranking score into a causal probability.
- Keep read-only diagnostics separate from actions that change a robot.
- Keep examples small enough to run in CI and inspect in a code review.

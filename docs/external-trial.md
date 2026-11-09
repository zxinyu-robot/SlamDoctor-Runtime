# External Trial Guide

This guide is for a developer who has not used SlamDoctor before. The first
trial should take about ten minutes and does not require ROS, a robot, or a
vendor SDK.

## 1. Install

```bash
git clone https://github.com/zxinyu-robot/SlamDoctor.git
cd SlamDoctor
python -m pip install ".[dev]"
```

Python 3.10 or newer is required.

## 2. Run the examples

List the built-in symptoms:

```bash
slam-doctor symptoms
```

Inspect the sample observations:

```bash
slam-doctor inspect examples/nav_oscillation.jsonl
```

Diagnose the sample:

```bash
slam-doctor diagnose examples/nav_oscillation.jsonl \
  --symptom NAV_OSCILLATION
```

Run the automated checks:

```bash
pytest
```

## 3. Evaluate the result

Please check whether you can answer these questions without additional
explanation:

1. What symptom was detected?
2. Which observations support each ranked hypothesis?
3. What evidence is missing?
4. What is the next low-cost probe?
5. Which conclusions are deliberately not claimed?

The score ranks hypotheses for investigation. It is not a calibrated causal
probability, and one anomaly does not establish a unique root cause.

## 4. Send feedback

Open an issue using the **External trial feedback** template. Please include:

- Operating system and Python version.
- The commands you ran.
- What worked and what was confusing.
- Whether the output was actionable.
- A sanitized example or a description of the data shape, if relevant.

Do not upload production logs, credentials, personal data, or other sensitive
information.

# Contributing to CANDID

Thank you for your interest in contributing to CANDID (Consumer Audits of Nudges & Deceptive Interface Design). This project exists to make the internet more honest for consumers, and community contributions are essential to that mission.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Adding a New Dark Pattern Test](#adding-a-new-dark-pattern-test)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Enhancements](#suggesting-enhancements)
- [Publishing Standards](#publishing-standards)
- [Development Setup](#development-setup)
- [Pull Request Process](#pull-request-process)
- [Style Guide](#style-guide)
- [Trademark Notice](#trademark-notice)

## Code of Conduct

This project follows our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold it. Please report unacceptable behavior to [candid-conduct@rahulsanjiv.dev](mailto:candid-conduct@rahulsanjiv.dev).

## How to Contribute

There are many ways to contribute:

| Contribution | Description |
|---|---|
| Add a new test | Write a Playwright-based test for one of India's 13 banned dark patterns |
| Improve an existing test | Reduce false positives or handle edge cases |
| Report a bug | File an issue with evidence for a test that gives wrong results |
| Improve documentation | Fix typos, clarify instructions, or translate docs |
| Build the report site | Frontend contributions to the public report card website |
| Validate findings | Manually verify automated test results against real app behavior |

## Adding a New Dark Pattern Test

This is the highest-impact contribution.

### 1. Pick a dark pattern from the list

India's CCPA guidelines define 13 dark patterns. Check the [open issues](../../issues) tagged `new-test` to see which ones still need coverage.

### 2. Write the test script

Create a new file in `candid/tests/`:

```python
"""
Test: [Dark Pattern Name]
CCPA Category: [e.g., "False Urgency", "Basket Sneaking"]
Description: [What this test checks, in one sentence]
"""

from candid.evidence.capture import CaptureSession


async def run(page, url: str, session: CaptureSession) -> dict:
    """
    Run the dark-pattern test.
    
    Args:
        page: Playwright page object
        url: Target URL to test
        session: Evidence capture session (screenshots, recordings)
    
    Returns:
        dict with keys:
            - "detected": bool. True if the dark pattern was found.
            - "confidence": float 0.0–1.0
            - "evidence": list of evidence file paths
            - "details": str. A human-readable explanation.
    """
    # Your test logic here
    await session.screenshot("initial_state")
    
    # ... test steps ...
    
    return {
        "detected": False,
        "confidence": 0.0,
        "evidence": session.artifacts(),
        "details": "No dark pattern detected."
    }
```

### 3. Write unit tests

Add tests in `tests/test_<pattern_name>.py` that verify your test script against known-good and known-bad mock pages.

### 4. Validate by hand

Before submitting, run your test against at least 2 real apps and compare the automated result with what you see manually. Document this in your PR.

## Reporting Bugs

File an issue with:

1. What you tested (URL, dark pattern, date)
2. What CANDID reported (the automated result)
3. What you actually saw (manual observation)
4. Screenshots of both the CANDID result and the real app behavior

## Suggesting Enhancements

Open an issue tagged `enhancement`. Describe:

1. The problem your enhancement solves
2. Who benefits (shoppers, researchers, regulators, companies)
3. How you would implement it (if you have ideas)

## Publishing Standards

> This is a critical section. CANDID's credibility depends on accuracy.

### Rules for publishing findings about named companies

1. No result about a named company is published without review. At least one maintainer must verify the automated finding against manual observation.
2. Every finding must have dated evidence. Screenshots, recordings, or HAR files with timestamps.
3. Companies get a right of reply. Before any public report names a company, we notify them and give them 7 calendar days to respond or fix the issue.
4. State objective observations. Say "CANDID detected that the countdown timer reset to 23:59 after page reload on [date]". Do not accuse companies of scamming users.
5. Retractions are public. If a finding is shown to be wrong, we publish a correction with equal visibility.

### What we never do

- Make real purchases or transactions
- Bypass authentication or access private data
- Reverse-engineer proprietary code
- Publish findings without evidence
- Claim legal conclusions (e.g., "this violates the law"). State what we observed.

## Development Setup

```bash
# Clone the repo
git clone https://github.com/rahulsanjiv-r/CANDID.git
cd CANDID

# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # Linux/Mac
# .venv\Scripts\activate   # Windows

# Install dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt  # linting, testing tools

# Install browser for Playwright
playwright install chromium

# Run tests
pytest tests/ -v
```

## Pull Request Process

1. Fork the repository and create your branch from `main`.
2. Write tests for any new functionality.
3. Run the full test suite locally: `pytest tests/ -v`
4. Lint your code: `ruff check .` and `ruff format .`
5. Update documentation if you changed any public-facing behavior.
6. Open a PR with a clear description of what changed and why.
7. A maintainer will review your PR. For test scripts, expect a manual validation step.

### PR title format

```
[test] Add fake-urgency countdown timer detection
[fix] Reduce false positives in hidden-cost test
[docs] Add setup instructions for Windows
[site] Add comparison view to report cards
```

## Style Guide

- Python: Follow PEP 8. Use type hints. We use `ruff` for linting and formatting.
- Naming: Test files are named after the dark pattern: `fake_urgency.py`, `hidden_costs.py`, `hard_to_cancel.py`.
- Comments: Every test must have a module docstring explaining what it checks and how.
- Evidence: All evidence must be captured through the `CaptureSession` API to ensure consistent naming, timestamping, and hashing.

## Trademark Notice

The name CANDID, the CANDID logo, and associated branding are trademarked by the CANDID project maintainers. You may not use the CANDID name or branding to represent your own fork, modified version, or derivative work without explicit written permission. See [TRADEMARK.md](TRADEMARK.md) for full details.

You are free to use, modify, and distribute the code under the AGPL-3.0 license, but you must use a different name and branding for any public-facing derivative.

---

Thank you for helping make the internet more honest.

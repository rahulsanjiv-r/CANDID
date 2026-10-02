<p align="center">
  <h1 align="center">CANDID</h1>
  <p align="center"><strong>Consumer Audits of Nudges & Deceptive Interface Design</strong></p>
  <p align="center">An independent, automated honesty check for apps and websites.</p>
</p>

<p align="center">
  <a href="#problem">Problem</a> •
  <a href="#how-it-works">How It Works</a> •
  <a href="#who-its-for">Who It's For</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#contributing">Contributing</a> •
  <a href="#license">License</a>
</p>

---

## Problem

When you shop or subscribe on an app in India, you can't tell whether it's tricking you.

- A countdown timer that restarts every time you reload the page.
- Extra charges that only appear at the very last checkout step.
- A "cancel subscription" button hidden five screens deep.

India's Consumer Protection Act and CCPA guidelines ban **13 dark patterns**. Platforms were asked to check *themselves*, and the guidelines include no way to verify their claims. **The company being judged is also the one writing the report.**

CANDID removes the self-reporting loop entirely.

It builds an **independent, public honesty check** for apps. It tests what apps *actually do* and publishes the proof, instead of trusting what they *say*.

## How It Works

1. **A robot visitor** opens the app or website like a normal customer would.
2. **It follows a fixed script.** It reloads the page to see if the timer resets. It compares the price on the product page with the price at checkout. It counts how many taps it takes to subscribe versus how many it takes to cancel.
3. **It records everything**, including screenshots, recordings, and timestamps. It compares what it found with what the company claimed.
4. **It publishes the result** with the dated evidence.
5. **The robot never buys anything.**

### What the first version tests

| # | Dark Pattern | How CANDID Tests It |
|---|---|---|
| 1 | **Fake Urgency** (countdown timers) | Reload the page 3 times. If the timer resets to the same value, it's fake. |
| 2 | **Hidden Costs** (drip pricing) | Compare the price on the product page with the final price at checkout. Flag if they differ and the extra charges were not visible upfront. |
| 3 | **Hard to Cancel** (roach motel) | Count the number of clicks/screens to subscribe. Count the number to cancel. Flag if cancelling takes 3× more steps than subscribing. |

## Who It's For

| User | What they get |
|---|---|
| **A shopper** | Search an app's name on the CANDID website and see a simple report card: *"Fake countdown timer: ❌ Found. Hidden fees at checkout: ❌ Found. Easy cancellation: ✅ Passed."* Each finding has a screenshot as proof. |
| **A journalist or researcher** | Compare apps side-by-side. See which ones got worse over time. Download evidence for reporting. |
| **A regulator or consumer court** | Get dated, reproducible evidence of what an app did on a given day. This is far stronger than a user complaint. |
| **A company** | See its own report, fix the problem, and request a re-test. Good companies want a clean public report. |

## Architecture (High Level)

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Test Runner │────▶│   Evidence   │────▶│  Report Card │
│  (Playwright)│     │    Store     │     │   Website    │
└──────────────┘     └──────────────┘     └──────────────┘
       │                    │                     │
       ▼                    ▼                     ▼
  Headless browser    Screenshots, videos,   Public-facing
  runs dark-pattern   HAR files, timestamps  per-app report
  test scripts        stored with hashes     cards + evidence
```

**Stack:**
- **Test runner:** Python + Playwright (headless browser automation)
- **Evidence store:** Local filesystem with SHA-256 hashes for integrity
- **Report site:** Static site (Next.js or plain HTML) displaying per-app results
- **CI/CD:** GitHub Actions for scheduled re-tests

## Getting Started

### Prerequisites

- Python 3.11+
- Node.js 18+ (for the report site)
- Git

### Installation

```bash
git clone https://github.com/rahulsanjiv-r/CANDID.git
cd CANDID
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
playwright install chromium
```

### Run a test

```bash
# Run the fake-urgency test against a URL
python -m candid.runner --test fake_urgency --url "https://example-shop.com/product/123"

# Run all 3 core tests against a URL
python -m candid.runner --test all --url "https://example-shop.com/product/123"
```

### View the report

```bash
# Generate the static report site
python -m candid.report generate

# Serve it locally
python -m candid.report serve
# → Open http://localhost:8080
```

## Project Structure

```
CANDID/
├── candid/                  # Core Python package
│   ├── __init__.py
│   ├── runner.py            # Test orchestrator
│   ├── tests/               # Dark pattern test scripts
│   │   ├── fake_urgency.py
│   │   ├── hidden_costs.py
│   │   └── hard_to_cancel.py
│   ├── evidence/            # Evidence capture & hashing
│   │   ├── capture.py
│   │   └── integrity.py
│   └── report/              # Report generation
│       ├── generate.py
│       └── serve.py
├── tests/                   # Unit & integration tests
├── report-site/             # Static report card website
├── docs/                    # Documentation
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── workflows/
├── README.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── TRADEMARK.md
├── LICENSE                  # AGPL-3.0
└── requirements.txt
```

## Honest Limits

We believe in stating what this tool can and cannot do:

- **Some tricks are subjective.** Guilt-tripping wording ("Are you sure you want to miss out?") is a matter of judgment and harder to test automatically. CANDID focuses on objectively measurable patterns first.
- **Apps change fast.** Every CANDID report carries a date. Results may not reflect today's version of an app. Re-testing is built into the workflow.
- **We publish observations, not accusations.** Every finding is backed by evidence (screenshot, recording, timestamp). Companies are given a right of reply before publication.
- **Legal limits apply.** CANDID never makes real purchases, never bypasses authentication, and never accesses private data. We follow responsible disclosure principles.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add new tests, report bugs, or improve the project.

All contributions require review before any result involving a named company is published.

## License

- **Code:** [GNU Affero General Public License v3.0 (AGPL-3.0)](LICENSE). You can view, use, and modify the code, but any modifications must also be shared under the same license.
- **Evidence & Reports:** [Creative Commons Attribution 4.0 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). Journalists, researchers, and regulators can reuse evidence with credit.
- **Name & Branding:** The name "CANDID", the logo, and associated branding are **trademarked** and may not be used without written permission. See [TRADEMARK.md](TRADEMARK.md).


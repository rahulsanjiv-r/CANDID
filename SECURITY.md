# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 0.x     | :white_check_mark: |

## Reporting a Vulnerability

Report security vulnerabilities in the CANDID codebase directly to us.

### How to Report

Do not open a public GitHub issue for security vulnerabilities.

Instead, email us at:

📧 candid-security@rahulsanjiv.dev

Include:

1. Description of the vulnerability
2. Steps to reproduce or a proof of concept
3. Impact assessment and what an attacker could do
4. Suggested fix, if available

### What to Expect

| Timeline | Action |
|---|---|
| 24 hours | We acknowledge receipt of your report |
| 72 hours | We provide an initial assessment and severity rating |
| 7 days | We aim to have a fix ready for critical vulnerabilities |
| 30 days | We aim to have a fix ready for non-critical vulnerabilities |

### What Counts as a Security Issue

- Evidence tampering: Any way to modify, delete, or forge evidence (screenshots, timestamps, hashes) after capture.
- Test manipulation: Making CANDID produce false results by exploiting the test framework.
- Authentication bypass: Relevant for future versions with user accounts.
- Dependency vulnerabilities: Critical CVEs in dependencies.
- Information disclosure: Leaking internal configuration, credentials, or unpublished findings.

### What Does NOT Count

- Dark patterns detected in tested apps. These are findings. Report them as regular issues.
- Disagreements about test methodology. File a regular issue or discussion.
- Companies blocking automated tests. This is expected behavior.

## Security Design Principles

CANDID's architecture follows these security principles:

1. Evidence integrity. All captured evidence is SHA-256 hashed at capture time. The hash is stored separately from the evidence file. Any modification is detectable.
2. No real transactions. CANDID never completes purchases, enters real payment information, or creates real accounts. The robot visitor is read-only.
3. No credential storage. CANDID does not store login credentials for tested apps. Tests operate on publicly accessible pages only.
4. Minimal permissions. The test runner operates with the minimum browser permissions necessary. It has no access to the local filesystem, camera, microphone, or location beyond what the test script explicitly requires.
5. Reproducibility over secrecy. Test scripts are public. We rely on behavioral variation like randomized timing, viewport sizes, and user agents rather than secrecy to reduce gaming by tested apps.

## Responsible Disclosure

We follow a coordinated disclosure process:

1. Reporter submits the vulnerability privately.
2. We confirm and assess severity.
3. We develop and test a fix.
4. We release the fix and credit the reporter.
5. We publish a security advisory after the fix is available.

We do not take legal action against security researchers who follow this coordinated disclosure process.

## Hall of Thanks

We publicly thank security researchers who help make CANDID more secure:

*No reports yet. Be the first.*

---

Thank you for helping keep CANDID secure. 🔒

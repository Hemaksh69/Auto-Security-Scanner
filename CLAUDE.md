# Auto-Security-Scanner Guide for Claude

## Project Purpose
A zero-dependency, heuristic security scanner for Unix-based systems. Designed for sub-second CI/CD performance and extensible threat detection.

## Technical Context
- **Primary Language:** POSIX-compliant Bash 4.0+
- **Key Dependencies:** `nmap`, `curl`, `jq` (all optional, degrades gracefully)
- **Architecture:** Modular pipeline (Input -> Detection -> Scoring -> Output)

## Coding Standards for Claude
- Use `shellcheck` (SC2034, etc.) for all changes.
- Ensure all detection modules emit valid JSON to `stdout`.
- Maintain idempotency; never mutate system state during a scan.
- For new checks, prioritize low-latency tools over heavy binaries.

## AI Integration Vision
When performing multi-file refactors, prioritize context-aware remediation that aligns with CIS benchmarks and NIST 800-53 controls.

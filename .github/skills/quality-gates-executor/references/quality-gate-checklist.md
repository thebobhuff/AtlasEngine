# Quality Gate Checklist

## Required If Relevant
- unit or integration tests
- lint
- typecheck
- static analysis
- security scan
- performance check
- accessibility check for UI work

## Do Not Mark Not Applicable Without Rationale For
- security scan on externally exposed or sensitive changes
- performance validation on latency-sensitive or high-load changes
- accessibility checks on user-facing changes

## Wave Pass Rule
- all relevant gates passed
- or explicitly not applicable with rationale
- and retry budget not exhausted from unresolved failures

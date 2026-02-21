# Security Gates

This repository uses a baseline security gate model for skill quality and operational readiness.

## Gate 1: Design and Authorization

1. Threat model exists for user, tenant, and admin paths.
2. Every write path has trusted-server authorization checks.
3. Row security rules are validated with negative tests.
4. Cross-tenant access tests fail for all protected resources.

## Gate 2: Secrets and Keys

1. API keys are server-only and scoped per service and environment.
2. Key rotation runbook is tested and repeatable.
3. Secret leak response flow exists and is tested.

## Gate 3: Dependency and Supply Chain

1. Dependabot is enabled for GitHub Actions updates.
2. Dependency review runs on pull requests.
3. Vulnerability scanning runs on pull requests, pushes, and schedule.
4. Secret scanning runs continuously in CI.

## Gate 4: Operations and Response

1. Activity and audit logs are monitored and mirrored externally.
2. Alert routing for abuse, auth failures, and permission anomalies is active.
3. Incident runbooks exist for key leak, account takeover, and data exposure.
4. Security regression checks run before release.

## Readiness Scoring

Use this scale after evidence-backed validation:

1. 0-49: High risk
2. 50-74: Partial controls
3. 75-89: Strong controls with known gaps
4. 90+: Strong controls with proven operating discipline

Scores are confidence indicators, not absolute safety guarantees.

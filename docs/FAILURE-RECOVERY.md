# Failure Recovery and Bounded Retry Policy

## States
`PENDING → READY → RUNNING → VALIDATING → COMPLETED`
Failure states: `RETRY_WAIT`, `ESCALATED`, `BLOCKED`, `FAILED`, `CANCELLED`.

## Classify before retry
- Transient: timeout, rate limit, temporary network/service issue.
- Environment/tooling: missing dependency, unavailable command, invalid credentials.
- Implementation: syntax, logic, integration or runtime defect.
- Test/design: invalid expectation, missing requirement, flawed contract.
- Data/model: schema/data quality, training/evaluation, drift or model regression.
- Security/policy: vulnerability, unauthorized scope or unsafe action.
- Requirement: ambiguity, conflict or changed scope.

## Suggested initial limits
- Small-tier model attempts: maximum 2.
- Mid-tier model attempts: maximum 2.
- High-tier model attempts: maximum 1 before human review.
- Total model attempts per task: maximum 5.
- Infrastructure retries: separate bounded exponential backoff with jitter and `Retry-After` support.
These are starting defaults and should be configurable per task class.

## Recovery rules
1. Preserve exact error, logs, command, environment and prior attempts.
2. Do not repeat an identical attempt without changing the hypothesis, input or environment.
3. Escalate Small → Mid → High when task reasoning exceeds the current tier.
4. Route implementation failures to Debugging/Bug Fixing; requirement/design failures to the responsible upstream phase.
5. For regression failures, preserve the failing test and evidence.
6. Stop on security violations, missing authorization, budget exhaustion or repeated no-progress.
7. Ask for human intervention when retries are exhausted or the action is high-impact.
8. Never automatically retry non-idempotent external actions unless an idempotency key or safe recovery strategy exists.

## Loop prevention
Enforce max_attempts, max_wall_time, max_cost, duplicate_error_signature detection, no_progress detection and cancellation. Persist state so restarts do not reset retry counts.

## Human approval
Required for production changes, destructive operations, database migrations, credential changes, external spending, security exceptions and model promotion.

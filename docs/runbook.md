---
category: runbook
title: Push Gateway runbook
description: On-call runbook for Push Gateway.
related_entities:
  - push-gateway
related_teams:
  - notifications
---

# Push Gateway runbook

On-call guide for `push-gateway` (Notifications, tier medium).

## Alerts

- **push-gateway-high-error-rate** — 5xx over 2% for 5m. Check upstream dependencies.
- **push-gateway-latency** — p99 over SLO. Check resource saturation.

## Common issues

- **Pod OOMKilled** — check memory limits and recent traffic spikes.
- **Crashloop** — check the last deploy and roll back if needed.

## Escalation

Page the Notifications on-call rotation.


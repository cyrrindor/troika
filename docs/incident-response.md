# Incident Response

## Severity Levels

| Level | Definition | Example | Initial Response SLA |
|---|---|---|---|
| **Sev1 — Critical** | Service is down or data loss is occurring | Production 500 errors, login broken, DB unreachable | 15 minutes |
| **Sev2 — High** | Core feature degraded, significant user impact | Checkout fails for some users, slow page loads > 10 s | 1 hour |
| **Sev3 — Low** | Non-critical feature broken, minor user impact | Minor UI bug, report generation slow | Next business day |

---

## Response Steps

### 1. Detect

Incidents are detected via:

- Vercel error rate alerts
- User reports in Slack `#support`
- Manual monitoring during deployments

### 2. Declare

If you suspect Sev1 or Sev2, immediately post in Slack `#incidents`:

```
🚨 INCIDENT: <one-line description>
Severity: Sev1 / Sev2
Detected: <time>
Impact: <who/what is affected>
Investigating: <your name>
```

Do not wait for full confirmation before declaring. Early declaration is always correct.

### 3. Assign an Incident Commander (IC)

For Sev1/Sev2, the first engineer to declare becomes IC until explicitly handed off. The IC coordinates the response — they do not have to fix the issue themselves.

IC responsibilities:
- Keep `#incidents` updated every 15 minutes
- Decide when to escalate to the CTO
- Call the all-clear and open the postmortem issue

### 4. Investigate

Typical investigation path:

1. Check recent deployments — was there a deploy in the last hour?
2. Check Vercel logs for error spikes
3. Check Supabase dashboard for DB health and slow queries
4. Roll back the last deployment if the issue started immediately after a deploy (Vercel dashboard → Deployments → Rollback)

### 5. Mitigate

Prefer the fastest mitigation over the cleanest fix:

- Rollback > hotfix > workaround > monitoring fix
- A rollback that restores service in 5 minutes beats a 2-hour hotfix

If rolling back: post in `#incidents` before and after.

### 6. Resolve

When the service is healthy again, post in `#incidents`:

```
✅ RESOLVED: <incident title>
Resolved at: <time>
Duration: <X minutes>
Root cause (preliminary): <one sentence>
Postmortem: will be filed within 48 hours
```

---

## Postmortem Process

Every Sev1 and Sev2 incident requires a postmortem. Sev3 incidents: optional but encouraged.

### When to File

Within **48 hours** of resolution, while the incident is fresh.

### Create a Postmortem Issue

Create a new issue in Paperclip (or GitHub) titled `Postmortem: <incident title>` with the following structure:

```markdown
## Summary

One paragraph: what happened, who was affected, for how long.

## Timeline

| Time | Event |
|---|---|
| HH:MM | Incident detected |
| HH:MM | Investigation started |
| HH:MM | Root cause identified |
| HH:MM | Mitigation applied |
| HH:MM | Service restored |

## Root Cause

What actually caused the incident. Be specific and blameless.

## Contributing Factors

List of conditions that allowed the incident to happen or made it worse.

## Impact

- Users affected: <number or percentage>
- Duration: <minutes/hours>
- Data loss: yes/no

## Action Items

| Action | Owner | Due |
|---|---|---|
| Add monitoring for X | @engineer | YYYY-MM-DD |
| Fix root cause Y | @engineer | YYYY-MM-DD |

## What Went Well

What helped us respond quickly or limit the blast radius.

## Lessons Learned

What we would do differently next time.
```

### Blameless Culture

Postmortems are about systems and processes, not individuals. The goal is to make the system more resilient — not to assign blame. Anyone who raises an issue in a postmortem is helping the team; never use postmortem data punitively.

---

## Escalation Path

```
Engineer on duty
      │ cannot resolve within SLA
      ▼
    CTO
      │ needs infrastructure/vendor access
      ▼
 Vercel / Supabase support
```

For Sev1 incidents that the on-duty engineer cannot resolve within 30 minutes, page the CTO directly via phone or Slack DM — do not wait for office hours.

---

## Communication During Incidents

- All incident communication in `#incidents` (not DMs)
- Update the thread at least every 15 minutes during active Sev1
- If a user asks about the outage: acknowledge it honestly, do not over-promise on resolution time
- Do not speculate about root cause publicly until the postmortem is filed

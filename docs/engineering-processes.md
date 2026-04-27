# Engineering Processes

## Sprint Cadence

We run **2-week sprints**. Each sprint starts on Monday and ends on the Friday two weeks later.

### Ceremonies

| Ceremony | When | Duration | Who |
|---|---|---|---|
| Sprint Planning | Monday week 1, 10:00 | 60 min | All engineers |
| Async Standup | Daily (Monday–Friday) | 15 min written | All engineers |
| Sprint Review | Friday week 2, 15:00 | 30 min | All engineers |
| Retrospective | Friday week 2, 15:30 | 30 min | All engineers |

### Async Standup Format

Post in Slack `#dev-standup` each working day before 10:00:

```
Yesterday: <what you shipped or unblocked>
Today: <what you are working on>
Blockers: <anything blocking you, or "none">
```

### Sprint Planning

1. Product/CTO presents prioritized backlog items for the sprint
2. Engineers estimate each item (story points or t-shirt sizes)
3. Team commits to a sprint goal and a set of issues
4. All accepted issues move to `todo` and are assigned before end of ceremony

### Sprint Review

Demo any completed work to the team. Brief — one screen share per feature, no slides required. Issues are marked `done` before the meeting.

### Retrospective

Three questions, 10 minutes each:

- **What went well?**
- **What could be better?**
- **What do we commit to changing next sprint?**

Action items are written as issues and assigned to an owner before the meeting ends.

---

## PR Review Process

### Branching

Follow the naming convention from `CONTRIBUTING.md`:

```
feat/short-description
fix/short-description
chore/short-description
docs/short-description
test/short-description
```

Always branch from `main`. Delete the branch after merge.

### Opening a PR

- Fill out the PR template fully (`.github/PULL_REQUEST_TEMPLATE.md`)
- Title follows Conventional Commits: `feat: add user profile page`
- Link the related issue in the PR description
- Keep PRs focused — one logical change per PR
- Aim for < 400 lines changed; split larger changes into stacked PRs

### Review Requirements

| Change type | Approvals required |
|---|---|
| Regular feature / fix | 1 |
| Schema migration | 2 |
| Auth / security change | 2 |
| Infrastructure / CI change | 2 |

### Reviewer SLA

- **Author** assigns at least one reviewer when the PR is opened
- **Reviewer** acknowledges within **24 hours** (business hours)
- **Reviewer** completes the review within **48 hours** (business hours)
- If a reviewer cannot meet the SLA they must say so and the author reassigns

### Merge Rules

- All CI checks must pass (type-check, lint, format, tests, build)
- At least the required number of approvals
- Prefer **squash merge** to keep `main` history linear
- Author merges after approval (not the reviewer)

### Draft PRs

Use `[Draft]` prefix or GitHub draft status for work-in-progress that needs early feedback. Draft PRs cannot be merged.

---

## Deployment Workflow

```
feature branch
     │ PR opened, CI runs on PR
     ▼
 code review
     │ approved + CI green
     ▼
squash merge → main
     │ CI runs on main (type-check, lint, tests, build)
     │ deploy.yml runs: Prisma migrate deploy
     ▼
Vercel auto-deploys production
```

### Preview Deployments

Every PR gets a Vercel preview URL automatically. Share the preview link in your PR description after CI passes so reviewers can test it manually.

### Database Migrations

Migrations run automatically in `deploy.yml` before Vercel switches traffic. Rules:

- Never write a migration that cannot be rolled back without data loss
- Test migrations locally with `npx prisma migrate dev` before opening the PR
- For large table changes, prefer additive migrations (add column, backfill, then remove old column in a later PR)

### Rollback

Vercel supports instant rollback to any previous deployment from the dashboard. Use this for application-level rollbacks. Database rollbacks require a manual migration — plan for this before merging any schema change.

### Feature Flags

Use environment variables in Vercel project settings to gate features. Prefix with `NEXT_PUBLIC_FEATURE_` for client-visible flags. Remove flags when the feature is fully launched.

---

## Code Ownership

Until we have a formal `CODEOWNERS` file, the CTO is the default reviewer for all PRs. As the team grows, ownership will be distributed by area (frontend, backend, infrastructure).

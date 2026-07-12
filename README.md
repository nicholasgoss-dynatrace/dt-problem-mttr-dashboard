# Problem MTTR Analysis — Dynatrace Dashboard

A ready-to-deploy Dynatrace dashboard for tracking **Mean Time to Resolve** (MTTR) for Davis AI-detected problems. Works on any Dynatrace SaaS or Managed environment with no configuration required.

## What It Shows

| Section | Tiles | Purpose |
|---------|-------|---------|
| **KPIs** | Closed Problems · Avg MTTR · Median MTTR · P95 MTTR | At-a-glance health of your remediation performance |
| **Resolution by Category** | MTTR bar chart · Problem donut chart | Which problem types take longest and occur most often |
| **Trends** | Weekly MTTR trend · Weekly volume trend | Are you getting better? Is problem frequency changing? |
| **Problem Detail** | Top root cause entities · Longest-duration problems | Where to focus remediation effort |
| **MTTR Detail Table** | Per-problem-name breakdown | Drill down to specific problem titles |

All tiles respect the **dashboard time picker** — set it to 30–90 days for meaningful trends. Only `CLOSED`, non-duplicate problems are included in MTTR calculations.

## Prerequisites

- [`dtctl`](https://github.com/nicholasgoss-dynatrace/dt-ai-obs-utils) CLI installed and configured

## Deploy

```bash
# Deploy to your active dtctl environment
dtctl apply -f dashboard.json -o yaml
```

The dashboard will be created as a new document in your Dynatrace environment. The output includes the dashboard URL.

## Update an Existing Deployment

If you've previously deployed this dashboard and want to pull in the latest version:

```bash
# 1. Download your current live version (preserves any UI edits)
dtctl get dashboard <your-dashboard-id> -o json --plain > my-dashboard.json

# 2. Manually merge in changes from dashboard.json, then:
dtctl apply -f my-dashboard.json -o yaml
```

## Problem Categories

Davis AI uses these `event.category` values:

| Category | Triggered by |
|----------|-------------|
| `AVAILABILITY` | Service unreachable, synthetic test failures, connection lost |
| `ERROR` | Increased error rates beyond baseline |
| `SLOWDOWN` | Response time degradation |
| `RESOURCE_CONTENTION` | CPU, memory, or disk saturation |
| `CUSTOM_ALERT` | Custom anomaly detection rules |

## Tips for SEs

- **No data?** Ensure the time picker covers a period with closed problems. The sprint demo environment may need 365-day range.
- **Explaining MTTR spike:** Click any data point to open the underlying problems for investigation.
- **Interpreting P95 vs Avg:** A large gap between Avg and P95 MTTR suggests a small number of very long-running incidents pulling the average up — worth investigating those outliers in the "Longest Duration Problems" table.
- **Root cause entities:** The "Top Root Cause Entities" tile works best on tenants where Davis has had time to identify root causes — typically needs Smartscape topology fully mapped.

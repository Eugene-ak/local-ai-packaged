# Freshservice Reporting Agent Setup

This workflow generates two report files:
- `freshservice-unresolved-YYYY-MM-DD.xlsx`
- `freshservice-summary-YYYY-MM-DD.doc`

The report includes:
- Unresolved tickets (`status:2` or `status:3`)
- SLA breached unresolved tickets (`sla_breached:true`)
- Ticket aging buckets (`0-2`, `3-7`, `8-14`, `15+` days)

## 1. Import the Workflow

In n8n:
1. Open **Workflows**.
2. Choose **Import from File**.
3. Import `n8n/backup/workflows/Freshservice_Reporting_Agent.json`.

## 2. Set Environment Variables

Add these to your n8n environment (for this repo, put them where your n8n service reads env vars):

- `FRESHSERVICE_DOMAIN` (example: `yourcompany.freshservice.com`)
- `FRESHSERVICE_API_KEY` (Freshservice API key)
- `FRESHSERVICE_REPORT_OUTPUT_DIR` (optional, default: `/data/shared/reports`)

If you use this repository's shared mount, `/data/shared/reports` maps to `shared/reports` in the host project.

## 3. Ensure Output Folder Exists

Create the folder on the host:

- `shared/reports`

## 4. Run It

You can trigger in two ways:
- **Manual Trigger** for on-demand reports
- **Weekday Schedule** (cron `0 8 * * 1-5`) for weekdays at 08:00

## 5. Notes and Hardening

- The template currently pulls page 1 only from Freshservice search results.
- If you have more than 100 unresolved tickets, add pagination in both HTTP nodes.
- You can refine the search query by team, group, priority, or requester.
- The `.doc` output is a Word-openable text report. If you need true `.docx`, add a document generation node (or an external conversion step).

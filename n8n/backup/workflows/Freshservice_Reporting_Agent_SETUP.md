# Freshservice Reporting Agent Setup

This workflow generates two report attachments and emails them:
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
- `REPORT_EMAIL_TO` (recipient email, example: `ops-team@company.com`)
- `REPORT_EMAIL_FROM` (sender email, example: `n8n-reports@company.com`)

## 3. Configure SMTP Credentials in n8n

The workflow uses n8n's **Send Email** node, so you must configure an SMTP credential:

1. In n8n, open **Credentials**.
2. Create **SMTP** credentials.
3. Enter your SMTP host, port, username, password, and TLS settings.
4. Save and attach this credential to both nodes:
	- `Email Excel Report`
	- `Email Word Summary`

## 4. Run It

You can trigger in two ways:
- **Manual Trigger** for on-demand reports
- **Weekday Schedule** (cron `0 8 * * 1-5`) for weekdays at 08:00

## 5. Notes and Hardening

- The workflow automatically paginates Freshservice tickets (100 per page) and keeps fetching until all pages are retrieved.
- A safety limit of 200 pages is applied in code to avoid infinite loops.
- You can refine the search query by team, group, priority, or requester.
- The `.doc` attachment is a Word-openable text report. If you need true `.docx`, add a document generation node (or an external conversion step).

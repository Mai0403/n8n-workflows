# n8n Workflows - Multi-Environment CI/CD

This repository manages n8n workflows across multiple environments (Development, Staging, Production) using a template-based approach with variable substitution.

## Repository Structure

```
n8n-workflows/
├── workflows/
│   ├── templates/           # Template workflows with {{VARIABLE}} placeholders
│   ├── dev/                 # Resolved dev workflows
│   ├── staging/             # Resolved staging workflows
│   └── production/          # Resolved production workflows
├── config/
│   ├── environments/        # Environment-specific ID mappings
│   │   ├── dev.yaml
│   │   ├── staging.yaml
│   │   └── production.yaml
│   └── mappings/            # Auto-generated ID mappings from backups
└── README.md
```

## How It Works

1. **Backup**: Export workflow from n8n → Templatize (replace IDs with `{{VARIABLE}}`) → Push to `workflows/templates/`
2. **Deploy**: Fetch template → Load environment config → Resolve variables → Import to target n8n instance

## Variable Placeholders

| Prefix | Purpose | Example |
|--------|---------|---------|
| `{{NODE_ID_*}}` | Node identifiers | `{{NODE_ID_WEBHOOK}}` |
| `{{CREDENTIAL_ID_*}}` | Credential references | `{{CREDENTIAL_ID_GOOGLE_SHEETS}}` |
| `{{WORKFLOW_ID_*}}` | Sub-workflow references | `{{WORKFLOW_ID_DATA_VALIDATION}}` |
| `{{GOOGLE_SHEET_ID}}` | External resources | Sheet/Doc IDs |
| `{{ENV_NAME}}` | Environment name | `dev`, `staging`, `production` |

## Branch Strategy

```
develop → main → staging → production
```

## Commit Convention

```
[ENV] Action: Description

Examples:
[TEMPLATE] Add: New validation step
[DEV] Fix: Webhook response format
[PROD] Deploy: v1.2.0
```

## Getting Started

1. Set up n8n CI/CD workflows in your n8n instance
2. Configure GitHub credentials in n8n
3. Update environment YAML files with actual IDs from your n8n instances
4. Use "Workflow Backup to GitHub" to backup workflows
5. Use "Retrieve Workflow from GitHub" to deploy to other environments

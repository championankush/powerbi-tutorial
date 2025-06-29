# PowerBI API & Automation 🤖

## Introduction

PowerBI REST API enables automation, integration, and DevOps for PowerBI solutions. This guide covers API usage, scripting, and best practices for CI/CD.

## 1. PowerBI REST API Overview
- Manage workspaces, datasets, reports, and users
- Automate deployments and refreshes
- Integrate with external systems

## 2. Authentication
- Register an Azure AD app for API access
- Use OAuth2 for authentication
- Grant required API permissions (Workspace, Dataset, Report)

## 3. Common API Scenarios
- List workspaces, reports, datasets
- Trigger dataset refresh
- Export reports as PDF/PPTX
- Add/remove users from workspaces

### Example: Trigger Dataset Refresh (Python)
```python
import requests
headers = {'Authorization': 'Bearer <access_token>'}
url = 'https://api.powerbi.com/v1.0/myorg/datasets/<dataset_id>/refreshes'
response = requests.post(url, headers=headers)
print(response.status_code)
```

## 4. Automation & DevOps
- Use PowerShell or Python scripts for automation
- Integrate with Azure DevOps for CI/CD pipelines
- Automate deployment of PBIX files, dataflows, and reports

### Example: PowerShell Automation
```powershell
# Install PowerBI module
Install-Module -Name MicrosoftPowerBIMgmt
# Login
Login-PowerBI
# Publish PBIX
Publish-PowerBIReport -Path 'report.pbix' -WorkspaceId '<workspace_id>'
```

## 5. Best Practices
- Use service principals for automation
- Securely store secrets (Azure Key Vault)
- Monitor API usage and quotas
- Document automation scripts

## Resources
- [PowerBI REST API Docs](https://docs.microsoft.com/en-us/rest/api/power-bi/)
- [PowerBI PowerShell](https://docs.microsoft.com/en-us/powershell/power-bi/overview)
- [CI/CD with PowerBI](https://docs.microsoft.com/en-us/power-bi/create-reports/deployment-pipelines)

---

**Automate and scale your PowerBI deployments with REST API and scripting!** 🤖 
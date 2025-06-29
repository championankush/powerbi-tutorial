# PowerBI Troubleshooting & FAQ 🛠️

## Introduction

This guide covers common PowerBI issues, troubleshooting steps, and frequently asked questions for both Desktop and Service.

## 1. Common Issues & Solutions

### Data Refresh Failures
- **Check data source credentials**
- **Verify gateway status** (for on-premises sources)
- **Check query folding in Power Query**
- **Review error messages in Service**

### Slow Report Performance
- **Optimize data model** (remove unused columns, use correct data types)
- **Optimize DAX measures** (use variables, avoid nested iterators)
- **Limit visuals and data points on report pages**
- **Use aggregations and summary tables**

### Visuals Not Displaying
- **Check field mappings**
- **Verify data types**
- **Ensure all required columns are present**
- **Update PowerBI Desktop to latest version**

### RLS Not Working
- **Test roles in Desktop**
- **Check user assignments in Service**
- **Verify DAX filter logic**

### Data Source Connection Issues
- **Check firewall and network settings**
- **Update drivers for databases**
- **Use correct connection strings**

## 2. Frequently Asked Questions

### How do I schedule data refresh?
- In PowerBI Service, go to dataset settings → Schedule refresh

### Can I use PowerBI for real-time dashboards?
- Yes, with DirectQuery, streaming datasets, or push APIs

### How do I share a report with external users?
- Use PowerBI Service sharing, or publish to web (with caution)

### What is the maximum dataset size?
- 1 GB for Pro, 400 GB for Premium per dataset (as of 2024)

### How do I get support?
- Use PowerBI Community, Microsoft Support, or your internal IT team

## 3. Resources
- [PowerBI Support](https://support.microsoft.com/en-us/powerbi)
- [PowerBI Community](https://community.powerbi.com/)
- [PowerBI Troubleshooting Docs](https://docs.microsoft.com/en-us/power-bi/troubleshoot/)

---

**Solve PowerBI issues quickly with this troubleshooting and FAQ guide!** 🛠️ 
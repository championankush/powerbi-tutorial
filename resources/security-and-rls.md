# PowerBI Security & Row-Level Security (RLS) 🔒

## Introduction

Security is a critical aspect of any PowerBI solution. This guide covers all major security features, including Row-Level Security (RLS), workspace permissions, data privacy, and best practices for secure deployments.

## 1. Row-Level Security (RLS)

### What is RLS?
Row-Level Security restricts data access for given users at the row level, ensuring users see only the data relevant to them.

### How to Implement RLS
1. **Define Roles** in PowerBI Desktop (Model View → Manage Roles)
2. **Create DAX Filters** for each role
   ```dax
   [Region] = USERPRINCIPALNAME()
   [Department] = "Sales"
   ```
3. **Test Roles** in Desktop (Model View → View as Roles)
4. **Publish to PowerBI Service**
5. **Assign Users** to roles in the Service (Dataset → Security)

### Example: Department-Based RLS
```dax
[Department] = LOOKUPVALUE(UserDepartments[Department], UserDepartments[User], USERPRINCIPALNAME())
```

### Dynamic RLS
- Use a mapping table (User, Department)
- Use `USERPRINCIPALNAME()` or `USERNAME()` in DAX

### RLS Best Practices
- Use dynamic RLS for scalability
- Document all roles and filters
- Test with real user accounts
- Avoid hardcoding usernames

## 2. Workspace Permissions

### Permission Levels
- **Admin**: Full control
- **Member**: Edit content
- **Contributor**: Add/edit content, no publish
- **Viewer**: Read-only access

### Assigning Permissions
- Use PowerBI Service workspace settings
- Assign users/groups to roles
- Use Azure AD groups for large organizations

## 3. Data Privacy & Sensitivity Labels

### Data Privacy Levels
- **Private**: Data is confidential
- **Organizational**: Data can be shared within org
- **Public**: Data can be shared externally

### Sensitivity Labels
- Set in PowerBI Service (requires Microsoft Information Protection)
- Labels: Confidential, Highly Confidential, General, etc.
- Enforce data loss prevention (DLP) policies

## 4. Secure Data Sources
- Use encrypted connections (SSL/TLS)
- Store credentials securely (Gateway, Azure Key Vault)
- Limit direct access to source databases

## 5. Auditing & Monitoring
- Enable audit logs in PowerBI Service
- Monitor access and sharing
- Use Microsoft Cloud App Security for advanced monitoring

## 6. Best Practices
- Principle of least privilege
- Regularly review permissions
- Use RLS for sensitive data
- Document security model
- Train users on data privacy

## Resources
- [PowerBI Security Documentation](https://docs.microsoft.com/en-us/power-bi/admin/service-security)
- [Row-Level Security in PowerBI](https://docs.microsoft.com/en-us/power-bi/admin/service-security-rls)
- [Data Protection in PowerBI](https://docs.microsoft.com/en-us/power-bi/admin/service-security-data-protection)

---

**Secure your PowerBI solutions to protect your data and users!** 🔒 
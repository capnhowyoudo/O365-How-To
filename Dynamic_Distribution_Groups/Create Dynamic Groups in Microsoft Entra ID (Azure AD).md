# Create Dynamic Groups in Microsoft Entra ID (Azure AD)

This guide explains how to create **Dynamic User Groups** in Microsoft Entra ID (formerly Azure Active Directory). Dynamic groups automatically add or remove users based on membership rules, eliminating the need to manually manage group membership.

This document includes examples for:

- Licensed users across multiple domains
- Licensed users from a single domain
- All licensed users

> **Note**
>
> Dynamic membership requires **Microsoft Entra ID P1 or P2** licensing.

---

# Prerequisites

Before creating a dynamic group, ensure you have:

- Microsoft Entra ID P1 or P2 licensing
- Group Administrator or Global Administrator permissions
- Access to the Microsoft Entra admin center
- Users with active Microsoft 365 licenses (if using licensed-user rules)

---

# Create a Dynamic Group

1. Sign in to the **Microsoft Entra admin center**. https://portal.azure.com/
2. Navigate to:

```text
Microsoft Entra ID
└── Groups
    └── New Group
```

3. Configure the group.

| Setting | Value |
|----------|-------|
| **Group Type** | Security or Microsoft 365 |
| **Group Name** | Enter a descriptive name |
| **Membership Type** | **Dynamic User** |

4. Select **Add Dynamic Query**.
5. Select **Edit**.
6. Paste one of the membership rules below.
7. Select **Save**.
8. Create the group.

---

# Dynamic Membership Rules

## Rule 1 – Licensed Users Across Multiple Domains

Use this rule to include **enabled users** with an active Microsoft 365 license whose User Principal Name (UPN) matches one of multiple domains.

> **Replace** **`domain1.com`** through **`domain6.com`** with your organization's domains.

```text
(user.accountEnabled -eq true) and
(user.assignedPlans -any
    (
        assignedPlan.servicePlanId -ne "" and
        assignedPlan.capabilityStatus -eq "Enabled"
    )
)
and
(
    user.userPrincipalName -match "^[A-Z0-9._%+-]+@domain1.com" or
    user.userPrincipalName -match "^[A-Z0-9._%+-]+@domain2.com" or
    user.userPrincipalName -match "^[A-Z0-9._%+-]+@domain3.com" or
    user.userPrincipalName -match "^[A-Z0-9._%+-]+@domain4.com" or
    user.userPrincipalName -match "^[A-Z0-9._%+-]+@domain5.com" or
    user.userPrincipalName -match "^[A-Z0-9._%+-]+@domain6.com"
)
```

---

## Rule 2 – Licensed Users from a Single Domain

Use this rule when your organization uses a single email domain.

> **Replace** **`domainname1.com`** with your organization's domain.

```text
(user.accountEnabled -eq true) and
(user.assignedPlans -any
    (
        assignedPlan.servicePlanId -ne "" and
        assignedPlan.capabilityStatus -eq "Enabled"
    )
)
and
(user.userPrincipalName -match "^[A-Z0-9._%+-]+@domainname1.com")
```

### Example

```text
(user.accountEnabled -eq true) and
(user.assignedPlans -any
    (
        assignedPlan.servicePlanId -ne "" and
        assignedPlan.capabilityStatus -eq "Enabled"
    )
)
and
(user.userPrincipalName -match "^[A-Z0-9._%+-]+@contoso.com")
```

---

## Rule 3 – All Licensed Users

Use this rule to include every enabled user with an active Microsoft 365 license, regardless of domain.

```text
(user.accountEnabled -eq true) and
(user.assignedPlans -any
    (
        assignedPlan.servicePlanId -ne "" and
        assignedPlan.capabilityStatus -eq "Enabled"
    )
)
```

---

# Rule Breakdown

## Enabled Accounts

```text
user.accountEnabled -eq true
```

Only includes user accounts that are enabled.

---

## Licensed Users

```text
(user.assignedPlans -any
    (
        assignedPlan.servicePlanId -ne "" and
        assignedPlan.capabilityStatus -eq "Enabled"
    )
)
```

Ensures the user has at least one enabled Microsoft 365 service plan assigned.

---

## Domain Filter

```text
user.userPrincipalName -match "^[A-Z0-9._%+-]+@domainname1.com"
```

Matches users whose **User Principal Name (UPN)** belongs to the specified domain.

Replace **`domainname1.com`** with your organization's email domain.

---

# Common Use Cases

| Scenario | Recommended Rule |
|-----------|------------------|
| Include all licensed users | Rule 3 |
| Include licensed users from one domain | Rule 2 |
| Include licensed users from multiple domains | Rule 1 |
| Multi-company or multi-brand Microsoft 365 tenant | Rule 1 |

---

# Best Practices

- Use **User Principal Name (UPN)** instead of proxy addresses or email aliases when filtering by domain.
- Use descriptive names for dynamic groups.
- Test membership rules before deploying to production.
- Allow time for Microsoft Entra ID to process dynamic membership changes.
- Periodically review and update membership rules as organizational requirements change.
- Document all dynamic membership rules for future administration.

---

# Suggested Naming Convention

| Purpose | Group Name |
|----------|------------|
| All Licensed Users | M365-Licensed-Users |
| Licensed Users (Single Domain) | M365-Company-Licensed |
| Licensed Users (Multiple Domains) | M365-MultiDomain-Licensed |
| Shared Services | M365-SharedServices |

---

# Troubleshooting

## Users Are Not Appearing in the Group

Verify the following:

- The user account is enabled.
- The user has an active Microsoft 365 license.
- The user's UPN matches the configured domain.
- The membership rule is valid.
- Dynamic membership processing has completed (this may take several minutes).

---

## Rule Validation

Before saving a dynamic membership rule:

- Verify all parentheses are balanced.
- Ensure quotation marks are correct.
- Confirm each `or` statement is properly formatted.
- Check that the domain names are spelled correctly.
- Use the **Validate Rules** option in the rule editor if available.

---

# References

- Microsoft Entra ID Dynamic Membership Rules
- Microsoft Entra ID Groups Documentation
- Microsoft 365 Licensing Documentation

---

## Version History

| Version | Date | Description |
|----------|------------|-------------|
| 1.0 | 2026-07-30 | Initial GitHub documentation |

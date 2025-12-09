# 1. Azure Governance Overview

Azure governance ensures that cloud environments are:
    Secure
    Compliant
    Organized
    Cost-controlled
    Consistent across all workloads

It is the foundation for enterprise-scale Azure architecture.

Governance includes:

    Management Groups
    Subscriptions
    Azure Policy
    RBAC
    Landing Zones
    Resource Organization (RGs, tags)

## 2. Management Groups (MGs)

Purpose:
Provide a hierarchy above subscriptions to organize and apply governance at scale.

Key facts:

    MG hierarchy can be up to 6 levels deep (excluding root).
    Policies and RBAC inherit downward.
    Subscriptions can belong to only one management group.
    Used for security boundaries, environment separation, billing, and compliance.
    Typical Enterprise MG Hierarchy:

```bash

Tenant Root Group
│
├── Platform
│   ├── Identity
│   ├── Management
│   └── Connectivity
│
├── Landing Zones
│   ├── Corp
│   └── Online
│
└── Sandbox
```
Exam tips:

    Apply policies at management group for consistency.
    Separate platform vs landing zones.
    Sandbox should be isolated with restrictive budgets.

## 3. Subscriptions

Subscriptions are billing boundaries and sometimes isolation boundaries.

When to use multiple subscriptions:

    Separation of environments (Prod / Dev)
    Separation of departments or cost centers
    Scalability or resource limits
    Security or blast radius isolation

Subscription design best practices:

    Use naming standards
    Use Azure Policy to enforce configurations
    Automate subscription creation (subscription vending) using:
    ARM/Bicep templates
    Azure Landing Zone automations

## 4. Azure Policy

Azure Policy enforces:

    Allowed configurations
    Compliance requirements
    Security baselines
    Tagging standards
    Resource restrictions

Key components:

    Policy Definition → what you want to enforce
    Initiative (Policy Set) → bundle of multiple policies
    Assignment → apply definition/initiative to MG / subscription / RG

Effects:

    Deny
    Audit
    Modify
    DeployIfNotExists (very important for exam)

Examples for AZ-305:

    Allow only specific VM SKUs
    Enforce resource location
    Enforce specific naming convention
    Enforce tags (env, costCenter)
    Deploy default diagnostics settings

Exam tips:

    Policy = enforcement, RBAC = permissions.
    Policy assignments inherit downward, just like RBAC.
    Use DeployIfNotExists for security baseline configurations.

## 5. Role-Based Access Control (RBAC)

    RBAC controls WHO can do WHAT on WHICH resource.

Scope levels:

    Management Group
    Subscription
    Resource Group
    Individual Resource
    Permissions always inherit downward.

RBAC object types:

    Users
    Groups (best practice for assignment)
    Service Principals
    Managed Identities
    Common built-in roles:
    Reader
    Contributor
    Owner (avoid assigning)
    User Access Administrator

Custom roles:

    Define if built-in roles are too broad.
    JSON-based.

Exam tips:

    Assign RBAC at highest appropriate scope.
    Avoid giving Owner privileges.
    Use Managed Identities instead of service principal secrets.

## 6. Azure Landing Zones

Landing Zones = enterprise-scale architecture frameworks for Azure.
Modern replacement for Azure Blueprints.

Core pillars:

    Identity & Access Management
    Network topology & connectivity
    Management & monitoring
    Security & compliance
    Cost management
    App Dev & DevOps

Types of landing zones:

    Platform Landing Zone → identity, networking, secure foundations
    Application Landing Zone → departments, workloads

Landing Zone benefits:

    Standardized resource organization
    Automated governance
    Consistent security baselines
    Ready for scaling across teams

Exam tips:

    Use landing zones for multi-subscription, multi-team envs.
    Governance and policies must be enforced before workloads are deployed.
    Use management groups to structure landing zones.

# 7. Azure Identity Overview

Azure Identity = authentication + authorization + identity lifecycle.

It includes:

    Azure AD (Entra ID)
    Users & groups
    App Registrations
    Service principals
    Managed identities
    Conditional Access
    Hybrid Identity

## 8. Azure Active Directory (Entra ID)
Core identity service for Azure.
Key identity objects:

    User
    Group
    Service Principal
    Managed Identity
    App Registration

| Feature                  | Service Principal | Managed Identity |
|--------------------------|-------------------|------------------|
| Requires secret/cert     | Yes               | No               |
| Auto-rotated credentials | No                | Yes              |
| Best use                 | External apps     | Azure resources  |

## 9. Authentication & Authorization

Authentication:

    OAuth 2.0
    OpenID Connect
    MSAL libraries

Authorization:

    RBAC for Azure resources
    App roles for application permissions

## 10. Conditional Access

Core to Zero Trust in Azure.
Used to enforce:

    MFA
    Device compliance
    Location requirements
    Risk-based controls

CA building blocks:

    Conditions
    Access controls
    Policies

Examples:

    Require MFA for users outside trusted locations
    Block legacy authentication
    Require compliant devices for sensitive apps

## 11. Hybrid Identity

Used when on-prem Active Directory still exists.
Components:

    Azure AD Connect Sync
    Password Hash Sync (PHS)
    Pass-through Authentication (PTA)
    Federation (AD FS)

When to use which:

    PHS → simplest, recommended
    PTA → if password hashes cannot sync
    Federation → only for highly regulated or legacy auth requirements

Exam tips:

    Prefer cloud-first identity unless requirements prevent it.
    Federation increases complexity — avoid in new designs.

## 12. Identity Governance (advanced)

Includes:

    Privileged Identity Management (PIM)
    Access reviews
    Entitlement management

Why identity governance matters:

    Reduces privilege creep
    Just-in-time access
    Enforces least privilege
    Meets compliance requirements


🎯 Exam Scenarios to Master (These ALWAYS appear)

Scenario 1
    “Ensure all resources have consistent tagging automatically.” → Use Azure Policy at MG level.

Scenario 2
    “Developers should deploy resources but cannot modify production.” → Use RBAC with Contributor role at Dev subscription.

Scenario 3
    “Ensure all new subscriptions follow security baseline automatically.” → Use Landing Zones + Policy initiatives at MG level.

Scenario 4
    “Application needs to access Key Vault without storing secrets.” → Use Managed Identity.

Scenario 5
    “Ensure MFA for risky sign-ins.” → Use Conditional Access.
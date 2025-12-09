# Azure Governance — General Notes (AZ-305)

Azure governance ensures that cloud environments remain:

- Secure  
- Compliant  
- Organized  
- Cost-controlled  
- Consistent across all workloads  

It is the foundation for enterprise-scale Azure architecture.

Governance includes:

- Management Groups (MGs)  
- Subscriptions  
- Azure Policy  
- RBAC  
- Landing Zones  
- Resource Organization (RGs, tags)

---

# 1. Management Groups (MGs)

## 📌 Purpose
Provide a hierarchy above subscriptions to organize and apply governance at scale.

## ✅ Key Facts
- MG hierarchy supports up to **6 levels** (excluding root).  
- **Policies and RBAC inherit downward** through the hierarchy.  
- A subscription can belong to **only one** management group.  
- Useful for security boundaries, environment separation, billing segmentation, and compliance.

## 🏗 Typical Enterprise MG Hierarchy
```
Tenant Root Group
│
├── Platform
│   ├── Identity
│   ├── Management
│   └── Connectivity
│
├── LandingZones
│   ├── Corp
│   └── Online
│
└── Sandbox
```

## 💡 Exam Tips
- Always apply **Azure Policy** at the **MG level** when possible.  
- Separate **Platform** (shared services) from **Landing Zones** (workloads).  
- Sandbox should be isolated and limited with budgets & policy restrictions.

---

# 2. Subscriptions

## 📌 Purpose
A subscription is a **billing boundary**, but also often an **isolation boundary**.

## 🧭 When to use multiple subscriptions
- Environment separation (**Prod / Dev / Test**)  
- Business units or cost centers  
- Scaling limits (Storage accounts, VNets, etc.)  
- Blast radius or security isolation  

## 🧩 Subscription Design Best Practices
- Use naming conventions (`corp-prod-sub`, `corp-dev-sub`)  
- Use Azure Policy to enforce governance  
- Automate subscription creation (“subscription vending”) using:
  - Bicep / ARM templates  
  - Azure Landing Zone automation  

---

# 3. Azure Policy

Azure Policy enforces:

- Allowed configurations  
- Compliance requirements  
- Security baselines  
- Tagging requirements  
- Resource restrictions  

## 🔧 Components
- **Policy Definition** → what to enforce  
- **Initiative (Policy Set)** → bundle of policies  
- **Assignment** → apply policy at MG / subscription / RG  

## ⚡ Policy Effects
- `Deny`  
- `Audit`  
- `Modify`  
- `DeployIfNotExists` *(very important for AZ-305)*

## 🧠 Examples Relevant to AZ-305
- Allow only specific VM SKUs  
- Restrict regions based on compliance  
- Enforce resource naming conventions  
- Apply mandatory tags (env, costCenter)  
- Enforce diagnostics settings automatically  

## 💡 Exam Tips
- **Policy = enforcement**, **RBAC = permission** → do not confuse.  
- Policy inheritance = RBAC inheritance.  
- `DeployIfNotExists` used for **security baselines** (e.g., enable logging).  

---

# 4. Role-Based Access Control (RBAC)

RBAC controls **who** can do **what** on **which** resource.

## 🔝 Scope Levels
1. Management Group  
2. Subscription  
3. Resource Group  
4. Resource  

**Permissions always inherit downward.**

## 👤 RBAC Object Types
- Users  
- Groups *(best practice)*  
- Service Principals  
- Managed Identities  

## 🏗 Common Built-In Roles
- Reader  
- Contributor  
- Owner *(avoid)*  
- User Access Administrator  

## 🔧 Custom Roles
- JSON-based  
- Used when built-in roles are too broad  

## 💡 Exam Tips
- Assign RBAC at the **highest appropriate scope**.  
- Avoid assigning **Owner** unnecessarily.  
- Prefer **Managed Identities** over service principals with secrets.  

---

# 5. Azure Landing Zones

Landing Zones are enterprise-scale architecture frameworks that define **how to structure Azure at scale**.

They replace Blueprints and follow Cloud Adoption Framework best practices.

## 🧱 Core Pillars
- Identity & access  
- Network topology & connectivity  
- Management & monitoring  
- Security & compliance  
- Cost management  
- Application Dev & DevOps  

## 🧩 Types of Landing Zones
- **Platform Landing Zone:** shared identity, network, management  
- **Application Landing Zone:** workloads or departments  

## 🎯 Benefits
- Standardized resource organization  
- Automated governance  
- Predefined security baselines  
- Consistent deployments across teams  

## 💡 Exam Tips
- Use Landing Zones for **multi-team** or **multi-subscription** environments.  
- Governance must be enforced **before** workload deployment.  
- Structure Landing Zones using **Management Groups**.  

---

# 6. Azure Identity Overview

Azure Identity includes:

- Azure AD (Entra ID)  
- Users & groups  
- App registrations  
- Service principals  
- Managed identities  
- Conditional Access  
- Hybrid identity  

---

# 7. Azure Active Directory (Entra ID)

## 🔑 Key Objects
- Users  
- Groups  
- Service Principals  
- Managed Identities  
- App Registrations  

## 🆚 Service Principal vs Managed Identity

| Feature                  | Service Principal | Managed Identity |
|--------------------------|-------------------|------------------|
| Requires secret/cert     | Yes               | No               |
| Auto-rotated creds       | No                | Yes              |
| Best use case            | External apps     | Azure resources  |

---

# 8. Authentication & Authorization

## 🔐 Authentication
- OAuth 2.0  
- OpenID Connect  
- MSAL Libraries  

## 🛡 Authorization
- RBAC for Azure resources  
- App roles for application security  

---

# 9. Conditional Access (CA)

Core to Zero Trust.

Used to enforce:

- MFA  
- Device compliance  
- Location restrictions  
- Risk-based controls  

## 🧱 CA Building Blocks
- Conditions  
- Grant controls  
- Policies  

## 📝 Examples
- Require MFA outside trusted locations  
- Block legacy authentication  
- Require compliant device for sensitive apps  

---

# 10. Hybrid Identity

Used when on-prem Active Directory still exists.

## 🔧 Components
- Azure AD Connect Sync  
- **Password Hash Sync (PHS)**  
- **Pass-through Authentication (PTA)**  
- **Federation (ADFS)**  

## ✔ When to use which
- **PHS** → simplest, recommended  
- **PTA** → password hashes must remain on-prem  
- **Federation** → smartcards, legacy apps, strict compliance  

## 💡 Exam Tips
- Prefer **cloud-first identity** unless unavoidable.  
- Avoid ADFS unless scenario demands it.  

---

# 11. Identity Governance (Advanced)

Includes:
- Privileged Identity Management (PIM)  
- Access Reviews  
- Entitlement Management  

Why it matters:
- Prevents privilege creep  
- Supports least privilege  
- Enables just-in-time access  
- Supports compliance frameworks  

---

# Exam Scenarios to Master

1. **“Ensure all resources have consistent tagging automatically.”**  
   → Use **Azure Policy** at MG level.

2. **“Developers should deploy resources but cannot modify production.”**  
   → Use **RBAC** (Contributor on Dev subscription).

3. **“Ensure new subscriptions follow security baseline automatically.”**  
   → Use **Landing Zones + Policy Initiatives**.

4. **“Application needs to access Key Vault without storing secrets.”**  
   → Use **Managed Identity**.

5. **“Ensure MFA for risky sign-ins.”**  
   → Use **Conditional Access + Identity Protection**.


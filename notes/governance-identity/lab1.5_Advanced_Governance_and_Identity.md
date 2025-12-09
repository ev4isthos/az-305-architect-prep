# AZ-305 — LAB 1.5: Advanced Governance & Identity
Completes the remaining topics from Unit 1: **Design identity, governance, and monitoring solutions**

---

## 🧭 Overview

**Goal:** Extend LAB 1 by covering the advanced identity and governance concepts required for AZ-305:

- Hybrid Identity  
- Conditional Access  
- Identity Protection  
- Privileged Identity Management (PIM)  
- Zero Trust  
- Governance at scale (Policy Initiatives & Landing Zones)

**Estimated Time:** 60–90 minutes

---

# 1. Hybrid Identity (Concept + Architecture Review)

## ⭐ Objective
Understand Azure AD + On-premises integration models, authentication methods, and when to choose each one.

Hybrid identity is **critical** for AZ-305 scenario design.

---

## 1.1 Review Hybrid Identity Architecture

Navigate to:

**Azure Portal → Azure Active Directory → Azure AD Connect → Hybrid Identity**

Study the architecture:

- On-prem AD DS  
- Azure AD  
- Sync Engine (AAD Connect)  
- Authentication Services  

---

## 1.2 Authentication Methods (Required Knowledge)

| Method | Use Case | Advantages | Limitations |
|--------|----------|------------|-------------|
| **Password Hash Sync (PHS)** | Default, most common | Simple, secure | Not real-time |
| **Pass-through Authentication (PTA)** | On-prem requirements | Real-time validation | Needs PTA agents |
| **Federation (ADFS)** | Advanced/legacy auth | Full control | Complex, costly |

### 💡 Exam Tips
- “Must validate passwords on-prem in real time” → **PTA**  
- “Smartcards, custom MFA” → **Federation (ADFS)**  
- “Simplest and recommended unless exceptions” → **PHS**

---

# 2. Conditional Access

## ⭐ Objective
Configure identity-based access rules such as MFA requirements.

---

## 2.1 Create a Conditional Access Policy

Path:  
**Azure AD → Security → Conditional Access → New Policy**

Configure:

- **Users:** Select a test account  
- **Cloud apps:** All cloud apps  
- **Conditions → Locations:** Exclude trusted locations  
- **Grant:** Require MFA  
- **Enable:** On  

Save and apply the policy.

---

## 2.2 Validate

Try logging in with your test user → MFA prompt should appear.

---

### 💡 Exam Tips
- “Block legacy authentication” → CA + disable legacy protocols  
- “Require MFA except from trusted locations” → CA with location condition  
- “Require compliant devices” → CA with device filters  

---

# 3. Identity Protection

## ⭐ Objective
Implement automated risk-based identity protection.

---

## 3.1 Configure User Risk Policy

**Azure AD → Security → Identity Protection → User risk policy**

- User risk: **High**  
- Control: Require password change  

---

## 3.2 Configure Sign-in Risk Policy

**Identity Protection → Sign-in risk policy**

- Sign-in risk: **Medium or high**  
- Control: Require MFA  

---

### 💡 Exam Tips
- “Automatically respond to risky sign-ins” → Identity Protection  
- “Force password reset when user risk is high” → User Risk Policy  
- “Protect against leaked credentials” → Identity Protection  

---

# 4. Privileged Identity Management (PIM)

## ⭐ Objective
Enforce Just-in-Time (JIT) access and remove standing admin rights.

---

## 4.1 Explore PIM Configuration

Path:  
**Azure AD → Privileged Identity Management**

Review concepts:

- Eligible vs active roles  
- Approval workflows  
- Time-limited role activation  
- MFA requirement for activation  

---

### 💡 Exam Tips
- “Minimize permanent admin privileges” → **Use PIM**  
- “Need approval for admin role activation” → **PIM workflow**  
- “Limit elevation to short time” → **PIM activation duration**  

---

# 5. Zero Trust Architecture

## ⭐ Objective
Understand how identity, policy, and network design follow Zero Trust principles.

---

## 5.1 Core Zero Trust Principles

| Principle | Azure Implementation |
|----------|----------------------|
| Verify explicitly | MFA, CA, Identity Protection |
| Least privilege | RBAC, PIM |
| Assume breach | NSGs, Firewall, Private Endpoints |

---

### 💡 Exam Tips
Scenarios mentioning:

- “Modern authentication model”  
- “Prevent lateral movement”  
- “Continuous verification”

→ The correct design aligns with **Zero Trust**.

---

# 6. Advanced Governance (Policy Initiatives + Landing Zones)

## ⭐ Objective
Apply governance at enterprise scale using Azure Policy and Landing Zones.

---

## 6.1 Create a Policy Initiative (Policy Set)

Path:  
**Azure Policy → Definitions → + Initiative Definition**

Add policies:

- Allowed locations  
- Allowed VM SKUs  
- Enforce mandatory tags  
- Enforce naming standards  

Name the initiative:

```
corp-governance-baseline
```

Assign it at the **Management Group** level.

---

## 6.2 Understand Azure Landing Zones

Study:  
**Azure Landing Zone Architecture**

Key components:

- Management group structure  
- Subscription separation  
- Policy-driven guardrails  
- Hub-and-spoke / Virtual WAN  
- Centralized Logging and Monitoring  

---

### 💡 Exam Tips
If scenario says:

- “Govern at scale”  
- “Ensure organizational compliance”  
- “Enforce standards across teams”

→ Correct answer = **Landing Zones + Policy Initiatives**

---

# ✔️ Completion Checklist

| Topic | Status |
|--------|---------|
| Hybrid Identity | ✅ |
| Authentication Models (PHS/PTA/ADFS) | ✅ |
| Conditional Access | ✅ |
| Identity Protection | ✅ |
| PIM | ✅ |
| Zero Trust | ✅ |
| Policy Initiatives | ✅ |
| Landing Zones | ✅ |

You now fully cover Unit 1 of AZ-305: **Governance and Identity.**

---

# 🎯 Next Step
Proceed to:

**LAB 2 — Network Design (Hub & Spoke, Azure Firewall, Private Endpoints)**


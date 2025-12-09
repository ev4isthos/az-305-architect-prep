# AZ-305 — LAB 1 — Governance Foundations  
### *Management Groups, Azure Policy, RBAC*

---

## 1️⃣ Create the Management Group Structure

### **Task**

Create the following Management Group (MG) hierarchy:

```
Tenant Root Group
│
├── Platform
└── LandingZones
    ├── Corp
    └── Online
```

---

### **Steps**

1. Go to **Azure Portal → Management Groups**  
2. Click **Start using management groups** if prompted  
3. Click **Create**  
4. Create these MGs:
   - `Platform`
   - `LandingZones`
   - `Corp`
   - `Online`
5. Drag & drop to arrange them into the hierarchy above.

---

### **Verification**

In the Management Groups view, the structure should look like the tree diagram.

---

## 2️⃣ Assign an Azure Policy (Tag Enforcement)

### **Goal**

Ensure every resource must include a `costCenter` tag.

---

### **Steps**

1. Go to **Azure Portal → Policy**  
2. Select **Definitions**  
3. Search for: **Require a tag and its value**  
4. Click **Assign**  
5. Choose scope: **LandingZones** Management Group  
6. Set:
   - Tag name: `costCenter`
   - Tag value: `required`
7. Click **Review + Create**

---

### **Verification**

Try deploying any resource *without the tag* (VM, Resource Group, Storage, etc.).

Deployment should **fail** with a policy violation.

---

## 3️⃣ Create a Custom RBAC Role

### **Goal**

Create a custom role that can read everything but can **only start/stop VMs**.

---

### **Steps**

1. Go to **Azure Portal → Subscriptions**  
2. Select your subscription  
3. Open **Access Control (IAM)**  
4. Go to **Roles → + Create**  
5. Choose **Custom role**

---

### **Define the Role JSON**

```json
{
  "properties": {
    "roleName": "VM Operator Limited",
    "description": "Can read resources and start/stop VMs only.",
    "permissions": [
      {
        "actions": [
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/deallocate/action",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Resources/subscriptions/resourceGroups/resources/read"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/<your-subscription-id>"
    ]
  }
}
```

---

### **Verification**

Assign the role to yourself at a **Resource Group** level.

You should be able to:

✔ Start VMs  
✔ Stop (deallocate) VMs  AZ-305 — 
✘ Cannot delete VMs  
✘ Cannot create new resources  

---

## 4️⃣ Assign RBAC at the Correct Scope

### **Scenario**

A developer must manage resources *only* in the **Corp** landing zone, not in **Online**.

---

### **Steps**

1. Go to **Management Groups → Corp**  
2. Open **Access Control (IAM)**  
3. Click **Add role assignment**  
4. Select **Contributor**  
5. Assign it to a test user (or secondary account)

---

### **Verification**

User should be able to:

✔ Deploy resources in **Corp**  
✘ Fail to deploy resources in **Online**

---

## 5️⃣ Lab Summary

In this lab you practiced:

- Creating a Management Group hierarchy  
- Applying governance using Azure Policy  
- Designing a custom RBAC role  
- Assigning permissions using best-practice scoping  

These are core skills tested heavily on **AZ-305**.

---

## 📚 References & Further Reading

### **Azure Governance**
- Management Groups  
  https://learn.microsoft.com/azure/governance/management-groups/overview  
- Resource Organization  
  https://learn.microsoft.com/azure/azure-resource-manager/management/overview  
- Landing Zone Architecture  
  https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/

### **Azure Policy**
- Policy Overview  
  https://learn.microsoft.com/azure/governance/policy/overview  
- Policy Effects  
  https://learn.microsoft.com/azure/governance/policy/concepts/effects  
- Built-in Policy Definitions  
  https://learn.microsoft.com/azure/governance/policy/samples/built-in-policies

### **RBAC**
- RBAC Overview  
  https://learn.microsoft.com/azure/role-based-access-control/overview  
- Built-in Roles  
  https://learn.microsoft.com/azure/role-based-access-control/built-in-roles  
- Custom Roles  
  https://learn.microsoft.com/azure/role-based-access-control/custom-roles  
- Assigning Roles  
  https://learn.microsoft.com/azure/role-based-access-control/role-assignments-portal
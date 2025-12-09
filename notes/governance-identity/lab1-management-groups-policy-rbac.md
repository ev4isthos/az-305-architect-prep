# 1️⃣ Create the Management Group Structure
Task

Create the following MG hierarchy:
```bash

Tenant Root Group
│
├── Platform
└── LandingZones
    ├── Corp
    └── Online
```
Steps

    1. Go to Azure Portal → Management Groups
   
    2. Click Start using management groups if you haven't before
   
    3. Click Create
   
    4. Create the following MGs:

    Platform
    LandingZones
    Corp
    Online

    5. Drag & drop to build the hierarchy.

Verification
In the portal, the MG screen should show the structure above.

# 2️⃣ Assign an Azure Policy (Tag Enforcement)

Ensure every resource must include a costCenter tag.
Steps

    1. Go to Azure Portal and then Policy
   
    2. Select Definitions
   
    3. Search for: “Require a tag and its value”

    4. Click Assign
   
    5. Choose scope: LandingZones MG

    6. Set tag name: costCenter

    7. Set tag value: required

    8. Review + Create

Verification
Create any resource (VM, RG, Storage) without the tag.
The deployment should fail with a policy error.

# 3️⃣ Create a Custom RBAC Role
Create a custom role that can read everything BUT only start/stop VMs.

Steps

    1. Go to Azure Portal → Subscriptions
   
    2. Select your subscription
   
    3. RBAC → Roles → + Create
   
    4. Choose Custom Role

Define JSON permissions:

```bash

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

Verification
Assign the role to yourself at a Resource Group level and confirm:

    You can start/stop VMs
    You cannot delete VMs
    You cannot create new resources

# 4️⃣ Assign RBAC at Correct Scope
Assign permissions using best practices.

Scenario
A developer needs to manage resources in Corp landing zone but NOT in Online.

Steps

    Go to Management Groups and then to Corp
    RBAC and then choose Add role assignment
    Select Contributor
    Assign to a test user or your second account.

Verification

User should:

    Deploy resources in the Corp subscription
    Fail to deploy in Online

# 5️⃣ Lab Summary
You now practiced:

    Creating a Management Group hierarchy
    Enforcing governance using Azure Policy
    Designing a custom RBAC role
    Assigning access at proper scopes

These are the core governance skills tested on AZ-305.
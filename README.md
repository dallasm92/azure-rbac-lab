# Azure RBAC Lab: Group-Based Access at Resource Group and Storage Account Scope

This lab documents a simple Azure role-based access control workflow using Microsoft Entra security groups, a resource group, and a storage account. The goal was to assign different permission levels at different scopes and confirm how inherited access appears in Azure IAM.

## Skills Demonstrated

- Creating and scoping Azure resources
- Creating Microsoft Entra security groups
- Assigning built-in Azure roles
- Distinguishing direct permissions from inherited permissions
- Applying least-privilege access design

## Technologies Used

- Microsoft Azure
- Microsoft Entra ID
- Azure Resource Manager
- Azure RBAC
- Azure Storage account IAM

## Environment / Scope

This lab was performed in:

- Subscription: `Azure subscription 1`
- Resource group: `rg-azure-lab-rbac`
- Storage account: `rbaclabstorage01`
- Storage replication shown in the build screenshots: `RA-GRS`
- Entra groups:
  - `Lab-Readers`
  - `Lab-Contributors`

## High-Level Lab Objectives

Create a resource group and storage account, create two Entra security groups, then assign:

- `Reader` to `Lab-Readers` at the resource-group scope
- `Contributor` to `Lab-Contributors` at the storage-account scope

The final goal was to confirm that:

- `Reader` access assigned at the resource group flows down to the storage account
- `Contributor` access assigned directly on the storage account stays specific to that resource

## Results Summary

The screenshots show this final state:

- `Lab-Readers` received a direct `Reader` assignment on `rg-azure-lab-rbac`
- `Lab-Contributors` received a direct `Contributor` assignment on `rbaclabstorage01`
- On the storage account IAM page, `Lab-Readers` appeared as `Reader` through inherited access from the resource group
- An `Owner` assignment for the operator account was also visible as inherited from the subscription, but it was not created as part of the lab workflow

## Screenshot Gallery

Key evidence from the lab:

| Step | Screenshot |
|---|---|
| Create the resource group | ![Create resource group](screenshots/01-resource-group-create.png) |
| Create the storage account | ![Create storage account](screenshots/03-storage-account-create-basics.png) |
| Confirm Entra groups exist | ![Entra groups created](screenshots/08-entra-groups-created.png) |
| Review the Reader role assignment | ![Reader role review](screenshots/10-reader-role-review-resource-group.png) |
| Final IAM state on the storage account | ![Final role assignments](screenshots/13-final-storage-account-role-assignments.png) |

## What I Learned

Azure RBAC becomes easier to reason about when the scope is deliberate. Assigning `Reader` at the resource-group level gives broad visibility across resources in that group, while assigning `Contributor` directly to a single storage account keeps change permissions narrower. The final IAM view makes the inheritance model easier to understand because Azure shows both the direct assignment and the inherited assignment together.

## Why This Matters

This is a basic but practical Azure administration pattern. In real environments, access is usually managed through groups rather than by assigning roles directly to individual users. That approach is easier to audit, easier to maintain, and better aligned with least-privilege access control.

## Notes

- The screenshots were copied into this repo from the original lab folder and the Azure account banner was redacted in the repo copies.
- This repo documents only what was visible in the screenshots. It does not assume extra steps that were not shown.
- The storage account uses `RA-GRS` in the captured build flow. That is more than was strictly needed for a low-cost beginner lab, but it is the configuration actually shown in the evidence and is documented as-is.

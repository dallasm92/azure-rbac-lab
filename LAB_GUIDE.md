# Azure RBAC Lab Guide

This walkthrough is based only on the screenshots included in this repo.

## 1. Create the resource group

The lab begins with a new resource group:

- Subscription: `Azure subscription 1`
- Resource group name: `rg-azure-lab-rbac`
- Region: `East US`

Evidence:

- [01-resource-group-create.png](screenshots/01-resource-group-create.png)
- [02-resource-group-created.png](screenshots/02-resource-group-created.png)

## 2. Create the storage account

The next step was to create a storage account inside the new resource group:

- Resource group: `rg-azure-lab-rbac`
- Storage account name: `rbaclabstorage01`
- Region: `East US`
- Performance: `Standard`
- Replication: `Read-access geo-redundant storage (RA-GRS)`

This replication choice is captured from the screenshots as built. It was not the cheapest possible option for a beginner lab, but the guide keeps the actual configuration rather than rewriting it into the originally intended low-cost path.

Evidence:

- [03-storage-account-create-basics.png](screenshots/03-storage-account-create-basics.png)
- [04-storage-account-review-create.png](screenshots/04-storage-account-review-create.png)
- [05-storage-account-deployment-complete.png](screenshots/05-storage-account-deployment-complete.png)

## 3. Create Microsoft Entra security groups

Two security groups were created for the access model:

### `Lab-Readers`

- Group type: `Security`
- Membership type: `Assigned`
- Description: `Reader access for Azure RBAC lab`

Evidence:

- [06-lab-readers-group-create.png](screenshots/06-lab-readers-group-create.png)

### `Lab-Contributors`

- Group type: `Security`
- Membership type: `Assigned`
- Description: `Contributor access for Azure RBAC lab`

Evidence:

- [07-lab-contributors-group-create.png](screenshots/07-lab-contributors-group-create.png)

### Confirm both groups exist

The group list shows both security groups after creation:

- `Lab-Contributors`
- `Lab-Readers`

Evidence:

- [08-entra-groups-created.png](screenshots/08-entra-groups-created.png)

The earlier tenant overview showed no groups before this step, which is consistent with this being a fresh RBAC lab setup rather than reuse of older directory groups.

## 4. Assign Reader at the resource-group scope

The resource group IAM blade was used to add a role assignment.

The screenshots show:

- Scope: `rg-azure-lab-rbac`
- Role selected: `Reader`
- Selected member: `Lab-Readers`

Evidence:

- [09-reader-role-selected-resource-group.png](screenshots/09-reader-role-selected-resource-group.png)
- [10-reader-role-review-resource-group.png](screenshots/10-reader-role-review-resource-group.png)

The review screen confirms the scope path points to the resource group.

After the assignment, the resource-group IAM page shows:

- `Lab-Readers` as `Reader`
- Scope: `This resource`

Evidence:

- [11-resource-group-role-assignments.png](screenshots/11-resource-group-role-assignments.png)

## 5. Assign Contributor at the storage-account scope

The workflow then moved to the storage account IAM page.

The screenshots show:

- Resource: `rbaclabstorage01`
- Role selected: `Contributor`
- Selected member: `Lab-Contributors`

Evidence:

- [12-contributor-role-review-storage-account.png](screenshots/12-contributor-role-review-storage-account.png)

The review screen confirms the full scope path points to:

- `rg-azure-lab-rbac/providers/Microsoft.Storage/storageAccounts/rbaclabstorage01`

That means the assignment was made directly on the storage account, not on the whole resource group.

## 6. Confirm the final role assignment state

The final storage-account IAM screen shows three visible assignments:

| Principal | Role | Scope status |
|---|---|---|
| Dallas Morison | Owner | `Subscription (Inherited)` |
| `Lab-Contributors` | Contributor | `This resource` |
| `Lab-Readers` | Reader | `Resource group (Inherited)` |

Evidence:

- [13-final-storage-account-role-assignments.png](screenshots/13-final-storage-account-role-assignments.png)

## Reader vs Contributor

`Reader` allows a principal to view resources and settings, but not make changes.

`Contributor` allows a principal to create, modify, and delete Azure resources within the assigned scope, but it does not grant permission to manage RBAC assignments themselves.

This matters in the lab because the two groups were intentionally separated:

- `Lab-Readers` was used for read-only visibility
- `Lab-Contributors` was used for change access on the storage account

## Scope Inheritance Explained

The screenshots show a simple inheritance pattern:

- A role assigned at the resource-group level applies to resources inside that group
- A role assigned directly to the storage account applies only to that storage account

That is why the final storage-account IAM page shows:

- `Lab-Readers` as `Reader` with `Resource group (Inherited)`
- `Lab-Contributors` as `Contributor` with `This resource`

## Final State

The final role model shown in the screenshots is:

- `Lab-Readers` has `Reader` on `rg-azure-lab-rbac`
- `Lab-Contributors` has `Contributor` on `rbaclabstorage01`
- the storage account displays both the direct assignment and the inherited assignment together

No extra assignments beyond what is shown in the screenshots are claimed here.

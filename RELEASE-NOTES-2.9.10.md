# CEDAR 2.9.10 Release

We’re pleased to announce CEDAR 2.9.10.

This release introduces an enhanced, more granular permission model for CEDAR artifacts and folders, together with redesigned permission and group-management interfaces. It also lays backend groundwork for future category-management capabilities.

## What’s New

### Enhanced, More Granular Roles for Artifacts and Folders

Earlier CEDAR releases offered two sharing permissions: Read and Write. Write combined content editing with higher-risk administrative actions such as changing access, moving resources, and managing OpenView. As a result, people sharing a resource had to grant more control than they intended when a collaborator needed only to edit content.

CEDAR 2.9.10 replaces that two-level model with three roles for templates, elements, fields, metadata instances, and folders:

* **Viewer** provides read access: the user can view an artifact or browse a folder.
* **Editor** adds content-management capabilities: the user can change content, create or copy resources into a folder, and delete resources, but cannot change who has access.
* **Manager** adds resource-administration capabilities: the user can manage access, move resources, and manage OpenView availability.

Editor includes every Viewer capability, and Manager includes every Editor capability. See the [CEDAR Permission Model](https://metadatacenter.readthedocs.io/en/latest/user-guide/advanced-topics/permission-model/) for the complete role, ownership, inheritance, and sharing rules.

Existing users do not lose permissions with this release. Existing Read grants become Viewer grants, while existing Write grants become Manager grants so that users retain every capability they previously had.

Ownership is now shown separately from a role. Owners retain full management capabilities and can transfer ownership to another user. This distinction makes it clearer whether access comes from ownership, a direct grant, group membership, the **Everyone** group, or an enclosing folder.

The built-in **Everyone** group can now receive only the Viewer role for new grants, preventing broad edit or management access from being assigned accidentally.

### Redesigned Sharing and Group Management

The sharing interface has been reorganized around the new Viewer, Editor, and Manager roles. It now:

* Explains what each role permits.
* Shows the owner separately from directly assigned roles.
* Allows authorized users to change or remove grants immediately.
* Allows an owner to transfer ownership to another user.
* Prevents groups from being selected as owners.

Group settings have also been redesigned to make creating, finding, and maintaining groups more direct. Group Administrators can edit group details and membership, assign other Group Administrators, and remove members. CEDAR now requires every group to retain at least one Group Administrator, preventing a group from becoming unmanageable.

## Preparatory Category Permission Work

CEDAR categories are named labels arranged in a hierarchy and used to classify artifacts—templates, elements, fields, and metadata instances. They let a community organize its artifacts under a shared structure, and let users browse that structure when finding relevant resources. A category can classify multiple artifacts, and an artifact can belong to multiple categories.

CEDAR 2.9.10 adds the backend authorization model needed for future category-management enhancements. Category access is represented by cumulative Viewer, Classifier, Editor, and Manager roles, separating the ability to browse a category, use it to classify artifacts, maintain the category hierarchy, and manage access to it.

This is preparatory infrastructure only. **CEDAR 2.9.10 does not add a category-management or category-sharing interface to the Workspace.** Existing category browsing and filtering remain available as before; user-facing category creation, maintenance, classification, sharing, and ownership workflows are planned for a later release.

## CEDAR Embeddable Editor 2.0.7

The wider CEDAR ecosystem also gains [CEDAR Embeddable Editor 2.0.7](https://github.com/metadatacenter/cedar-embeddable-editor/releases/tag/release-2.0.7). It improves temporal input validation, presents numeric field types more clearly, and updates the editor’s translation integration for Angular 22.

Build and test isolation has also been improved across the backend, and release tooling now performs stronger checks of frontend dependencies, lockfiles, toolchains, and build-train inputs.

## Important Notes for Integrators

* The CEDAR Resource REST API’s artifact and folder permission endpoints require `role` with `viewer`, `editor`, or `manager`. The former `permission` property and its `read` and `write` values are not accepted.
* Clients should use the returned `capabilities` and `availableActions` rather than infer authorization from a role name or ownership.
* Existing stored `CANREAD` and `CANWRITE` grants remain valid and are interpreted as Viewer and Manager respectively. The release does not require an immediate permission-data migration.
* Updating permissions or transferring ownership requires the current permission revision through `If-Match`; missing and stale revisions are rejected rather than overwriting newer changes.
* The category role and capability APIs are groundwork for later clients. Integrators should not present them as a complete end-user category workflow in this release.

For the complete technical change history, see the [comparison between CEDAR 2.9.9 and 2.9.10](https://github.com/metadatacenter/cedar-project/compare/release-2.9.9...release-2.9.10).

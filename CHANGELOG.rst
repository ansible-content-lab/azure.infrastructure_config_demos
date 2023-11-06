=========================================================
lab.azure.azure_infrastructure_config_demos Release Notes
=========================================================

.. contents:: Topics

This changelog describes changes after version 1.4.2.

v2.0.0
======

Release Summary
---------------

Updated the collection to match validated patterns.

Major Changes
-------------

- Moved playbooks into playbooks folder so that playbooks may be called from `namespace.collection.playbook` syntax.
- Updated Ansible Lint config.

Breaking Changes / Porting Guide
--------------------------------

- Playbook names have changed.
- Playbook paths have changed.
- Playbooks now reference role by FQRN.

v1.7.0
======

Release Summary
---------------

Moved playbooks into playbooks folder and updated dependency versions.

Major Changes
-------------

- Moved playbooks into playbooks folder so that playbooks may be called from `namespace.collection.playbook` syntax.
- Updated dependency versions.

Breaking Changes / Porting Guide
--------------------------------

- Playbook paths have changed.

v1.6.0
======

Release Summary
---------------

Added VM blueprints and simple VNET role.

Major Changes
-------------

- Added a VNET role to provide an example of simple VNET creation.

Minor Changes
-------------

- Readme updates

Breaking Changes / Porting Guide
--------------------------------

- Added and removed playbooks to account for role changes.
- Removed OS-specific playbook and role tasks and created a generic VM creation model based on VM blueprints.

v1.5.0
======

Release Summary
---------------

Added the release changelog.

Minor Changes
-------------

- Required Ansible Lint rule for change logs.

===============================================
Azure.Infrastructure_Config_Demos Release Notes
===============================================

.. contents:: Topics

This changelog describes changes after version 1.4.2.

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

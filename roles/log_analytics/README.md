# Log Analytics Role

This Ansible role contains repeatable automation for deploying an Azure Log Analytics workspace and then installs the Log Analytics agent onto machines that need to report into the workspace.

## Variables

The following variables are used when deploying or destroying a Log Analytics workspace.

### Required

```yaml
---
workspace_name: log-ws
resource_group_name: rg-logs
```

## Create Workspace Tasks

This set of tasks creates the Log Analytics workspace within your Azure subscription.

## Delete Workspace Tasks

This set of tasks destroys the Log Analytics workspace within your Azure subscription.

## Install Agent Linux Tasks

This set of tasks installs the Log Analytics agent on RHEL-derivative Linux distros and connects those clients to a Log Analytics workspace.  It assumes a host inventory group called `linux` to contain the hosts that will be automated against.

## Uninstall Agent Linux Tasks

This set of tasks uninstalls the Log Analytics agent from Linux-based hosts.  It assumes a host inventory group called `linux` to contain the hosts that will be automated against.
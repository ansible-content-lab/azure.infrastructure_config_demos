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

## Tasks

### Create Workspace

This set of tasks creates the Log Analytics workspace within your Azure subscription.

## Delete Workspace

This set of tasks destroys the Log Analytics workspace within your Azure subscription.

### Install Agent

This set of tasks installs the Log Analytics agent and connects those clients to a Log Analytics workspace.

### Uninstall Agent

This set of tasks uninstalls the Log Analytics agent from hosts.
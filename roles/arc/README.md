# Arc Role

This Ansible role contains repeatable automation for deploying and managing Azure Arc components.  This role relies heavily on the Azure CLI since many Azure Arc components are not exposed in the Ansible Azure collection yet.  The content in this role will be updated as the collection support improves.

## Variables

The following variables are used throughout the tasks in this role.  Defaults are set to manage Linux-based hosts, but the added comments show where the Windows equivalents can be used instead.

### Required

```yaml
---
extensions:
  azure_monitor_agent:
    name: AzureMonitorLinuxAgent # AzureMonitorWindowsAgent
    type: AzureMonitorLinuxAgent # AzureMonitorWindowsAgent
    publisher: Microsoft.Azure.Monitor
  log_analytics_agent:
    name: OmsAgentForLinux # MicrosoftMonitoringAgent
    type_handler_version: 1.13
    type: OmsAgentForLinux # MicrosoftMonitoringAgent
    publisher: Microsoft.EnterpriseCloud.Monitoring
```

## Tasks

### Install Agent

This set of tasks installs the Azure Arc agent and connects those clients to Azure.

### Disconnect Agent

This set of tasks uninstalls the Azure Arc agent from hosts.

### Enable Azure Monitor Agent Extension

Enables the Azure Monitor Agent extension within the Arc agent.  This is the modern logging and monitoring extension that will eventually replace the log analytics agent.

## Enable Log Analytics Agent Extension

Enables the Azure Log Analytics extension within the Arc agent.  This is a widely used logging agent for Azure connected servers, but Microsoft has announced its deprecation.

### Disable Log Analytics Extension

Disables and removes the Azure Log Analytics extension through the Arc connected machine process.  This is used as part of the playbook that migrates from the Log Analytics extension to the Azure Monitor Agent extension.
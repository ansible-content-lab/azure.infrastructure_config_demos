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

Installs the Azure Arc agent and connects those clients to Azure on each host.

### Disconnect Agent

Runs the disconnect operation from Azure Arc agent on each hosts.

### Enable Azure Monitor Agent Extension

Enables the Azure Monitor Agent extension within the Arc agent on each host.  This is the modern logging and monitoring extension that will eventually replace the log analytics agent.

### Enable AD SSH Login Extension

Enables the AD SSH Login extension within the Arc agent on each host.

## Enable Log Analytics Agent Extension

Enables the Azure Log Analytics extension within the Arc agent on each host.  This is a widely used logging agent for Azure connected servers, but Microsoft has announced its deprecation.

### Disable Extensions

Disables extensions provided as extra vars on each host.
# Building a SOC + Honeynet in Azure (Live Traffic)
<img src="Project diagram.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/austinabutech/HoneyPot_Azure/assets/163788570/b9122858-7880-4dd4-a0ad-886a0073a5ee)


## Architecture After Hardening / Security Controls
![image](https://github.com/austinabutech/HoneyPot_Azure/assets/163788570/cb125fbe-8c04-487a-ba83-a020f4bb90ec)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
<img src="Linux before.png" height="80%" width="80%" alt="Disk Sanitization Steps"/><br>
<img src="Windows before.png" height="80%" width="80%" alt="Disk Sanitization Steps"/><br><br>
<img src="MSSQ DB failed auth before.png" height="80%" width="80%" alt="Disk Sanitization Steps"/><br><br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 18 hours:
Start Time 2024-03-25 13:07
Stop Time 2024-03-26 8:14

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4459
| Syslog                   | 1355
| SecurityAlert            | 9
| SecurityIncident         | 395


## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-03-27 15:37
Stop Time	2024-03-279 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 6778
| Syslog                   | 27
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this endeavor, a miniature honeynet was established within the Microsoft Azure platform, with log sources seamlessly integrated into a Log Analytics workspace. Leveraging Microsoft Sentinel, alerts were effectively triggered and incidents promptly generated from the aggregated logs. Furthermore, comprehensive metrics were gathered within the vulnerable environment both prior to and subsequent to the implementation of security protocols. Significantly, the implementation of these security measures resulted in a substantial reduction in both the frequency of security events and incidents, clearly illustrating their efficacy.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

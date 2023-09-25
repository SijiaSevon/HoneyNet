# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

For this endeavor, I set up a compact honeynet on Azure. I channeled logs from multiple sources into a Log Analytics workspace. This information is utilized by Microsoft Sentinel to generate attack maps, initiate alerts, and establish incidents. I assessed particular security metrics in a less secure environment for 24 hours, implemented several security measures to bolster the environment, and gauged metrics again for another 24 hours. The results are presented below. The metrics we will discuss are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, resources were initially deployed and made accessible from the internet. The Virtual Machines had their Network Security Groups and inherent firewalls fully open, with all other resources having public endpoints accessible to the internet; in other words, no Private Endpoints were utilized.

For the "AFTER" metrics, Network Security Groups were reinforced by blocking ALL incoming and outgoing traffic, save for my administrative workstation. Additionally, all other resources were shielded by their native firewalls and the inclusion of Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19550
| Syslog                   | 3090
| SecurityAlert            | 12
| SecurityIncident         | 350
| AzureNetworkAnalytics_CL | 860

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-07-21 15:37
Stop Time	2023-07-22 15:39

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8810
| Syslog                   | 28
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0


## Conclusion

In this endeavor, a compact honeynet was set up in Microsoft Azure, and log data was channeled into a Log Analytics workspace. Microsoft Sentinel took on the role of initiating alerts and forming incidents based on these logs. Security metrics were first gauged in the vulnerable setting before implementing protective measures, and then re-evaluated post-implementation. The significant decrease in security events and incidents after the application of these measures underscores their efficacy.

It's important to highlight that had the network resources been frequently accessed by regular users, the number of security events and alerts might have seen a rise in the 24 hours subsequent to the deployment of the security controls.

# Building a SOC + Honeynet in Azure (Live Traffic)
<!--![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg) -->

## Introduction

Welcome to my Cloud SOC project! Here, I present the creation of a compact honeynet within Azure, combined with the aggregation of log sources into a Log Analytics workspace. These logs are leveraged by Microsoft Sentinel to construct attack maps, generate timely alerts, and initiate incident responses.

To evaluate the security posture within my insecure environment, I conducted measurements for 24 hours, I then implemented a series of security controls to harden the environment. After which I tested for another 24 hours. The following security metrics are showcased:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architencture Before Hardening](https://github.com/krzysztof-cloud/Azure-Honeynet-SOC/blob/main/architecture_before.png)

## Architecture After Hardening / Security Controls
![Architecture After Hardening](https://github.com/krzysztof-cloud/Azure-Honeynet-SOC/blob/main/architecture_after.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows, 1 linux)
- MS SQL Server running on the Windows VM
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
Here are some of the attack maps I've created in Sentinel to track the number of malicious flows allowed inbound to my Network security group, auth failures to my Linux VM, RDP auth failures to my Windows VM, as well as all auth failures to the SQL server hosted on my VM. <br><br>
![NSG Allowed Inbound Malicious Flows](https://github.com/krzysztof-cloud/Azure-Honeynet-SOC/blob/main/nsg-maliciousflows-allowed-in-24hrs-before.PNG?)<br>
![Linux Syslog Auth Failures](https://github.com/krzysztof-cloud/Azure-Honeynet-SOC/blob/main/syslog-linux-ssh-auth-fail-24hrs-before.PNG)<br>
![Windows RDP/SMB Auth Failures](https://github.com/krzysztof-cloud/Azure-Honeynet-SOC/blob/main/windows-rdp-auth-fail-24hrs-before.PNG)<br>
![MS SQL Auth Failures](https://github.com/krzysztof-cloud/Azure-Honeynet-SOC/blob/main/mssql-auth-fail-24hrs-before.PNG)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br><br>
Start Time 2023-06-29 13:36<br>
Stop Time 2023-06-30 13:45<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4358
| Syslog                   | 2345
| SecurityAlert            | 6 
| SecurityIncident         | 73
| AzureNetworkAnalytics_CL | 103

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:<br>

Start Time 2023-06-30 15:37<br>
Stop Time	2023-07-01 15:57<br>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 230
| Syslog                   | 12
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

This project involved building a mini honeynet in Microsoft Azure and integrating log sources into a Log Analytics workspace. By utilizing Microsoft Sentinel, alerts and incidents were automatically triggered based on the ingested logs. Furthermore, security metrics were measured in the initial insecure environment and after implementing security controls. Notably, the implementation of these controls resulted in a significant reduction in security events and incidents, serving as clear evidence of their effectiveness.

It is important to consider that if the network resources experienced substantial usage by regular users, it is possible that a higher number of security events and alerts could have been generated within the 24-hour period after implementing the security controls.

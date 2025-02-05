
# Building a SOC + Honeynet in Azure (Live Traffic)
![Screenshot 2023-05-30 021705](https://user-images.githubusercontent.com/65828628/236935173-6cb5f050-376a-4396-97aa-c147d9297f52.gif)



## Introduction

For this project, I constructed a miniature honeynet using Azure and established a Log Analytics workspace to gather log data from diverse sources. This workspace is utilized by Microsoft Sentinel to construct attack maps, activate alerts, and generate incidents. Initially, I assessed security metrics within an insecure environment for a duration of 24 hours. Subsequently, I implemented several security controls to fortify the environment, and measured the metrics for an additional 24-hour period. The outcomes of this evaluation are presented below, highlighting the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Objective
The central aim of this project was to create intentionally vulnerable virtual machines within the Azure infrastructure. This initiative served two purposes: first, to attract and analyze cyber attacks, providing valuable insights into the tactics and techniques utilized by attackers; second, to showcase my capability to swiftly and effectively respond to any identified issues.

## Regulations, and Azure Componets Deployed

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2x windows, 1x linux)
- Log Analytics Workspace
- Azure Key Vault for secure secret management 
- Azure Storage Account for data storage
- Microsoft Sentinel for Security Information and Event Management(SIEM)
- Windows Remote Desktop to remotely connect to virtual machines
- Microsoft Defender Cloud to protect Cloud resources 
- Command Line Interface (CLI) for system management 
- PowerShell for Automation and Configuration management 
- Kusto Query Language (KQL) 
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls 
- [NIST SP 800-61 Revision 2](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final) for Incident Hardening & Guidance 


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

In the initial stage of the project, referred to as the <b>"BEFORE"</b> stage, the resources were deployed without any hardening measures or security controls in place. The purpose of this setup was to intentionally create an insecure environment to attract potential cyber attackers and study their tactics. During this stage:

- 1.) <b>Public Exposure:</b> All resources were initially deployed with direct public exposure to the internet. This means that they were accessible from any source on the internet without any restrictions.

- 2.) <b>Network Security Groups (NSGs) and Firewalls:</b> The Virtual Machines had their Network Security Groups (NSGs) and built-in firewalls configured in a permissive manner. This allowed unrestricted access to and from the Virtual Machines, without any limitations or filtering of network traffic.

- 3.) <b>Public Endpoints:</b> Other resources, such as storage accounts and databases, were deployed with public endpoints that were visible and accessible from the internet. Private Endpoints, which provide a more secure and private connection to these resources, were not utilized during this stage.

The purpose of this insecure setup was to actively observe and analyze potential cyber attacks, their techniques, and the impact on the deployed resources. This allowed for a comprehensive understanding of the vulnerabilities and risks present in the initial environment.



## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

During the <b>"AFTER"</b> stage of the project, environment was hardened, and stringent security controls were implemented to enhance the security posture and satisfy NIST SP 800-53 Rev4 SC-7(3) Access Points. These improvements focused on robust hardening measures, including key highlights of the enhancements:

- 1.) <b>Reinforced Network Security Groups (NSGs):</b> I bolstered the NSGs by implementing stringent access rules. This ensured that only trusted sources, such as specific authorized IP addresses, were allowed to connect to the virtual machines. By limiting access to known entities, I mitigated the risk of unauthorized entry and potential attacks.

- 2.) <b>Enhanced Built-in Firewalls:</b> I fine-tuned the built-in firewalls on the virtual machines to establish a resilient defense mechanism. Through meticulous configuration, I reduced the attack surface and fortified the overall security posture. These firewalls acted as effective barriers, preventing unauthorized access and protecting our resources from potential threats.

- 3.) <b>Secure Private Endpoints:</b> To elevate the security of critical Azure resources, I transitioned from public endpoints to secure Private Endpoints. This strategic shift ensured that access to sensitive resources, such as storage accounts and databases, was limited to our internal network. By isolating these resources from the public internet, I significantly reduced the exposure to potential attacks, enhancing the overall privacy and protection.

By conducting a thorough comparison of the security metrics before and after the implementation of these robust hardening measures and stringent security controls, I demonstrated the effectiveness of these enhancements in significantly fortifying the security posture of the Azure environment. These measures not only mitigate risks but also instill confidence in safeguarding valuable data and resources.




## Attack Maps Before Hardening / Security Controls
![(Before) nsg-malicious-allowed ](https://github.com/ryanjustindejesus/Azure-SOC-Honey-Project/blob/main/nsg-malicious-allowed-in.png)


- The showcased attack map offers a visual depiction of the direct consequences that occur when the <b>Network Security Group (NSG)</b> is left unsecured, permitting unrestricted entry of malicious traffic. This compelling visualization strongly highlights the critical significance of implementing stringent security measures, including the enforcement of restrictions on NSG rules. Such measures play a pivotal role in proactively preventing unauthorized access and effectively minimizing potential threats to the system.


![(Before) syslog-ssh-auth-fail](https://github.com/ryanjustindejesus/Azure-SOC-Honey-Project/blob/main/linux-ssh-auth-fail.png)

- The highlighted attack map draws attention to a notable occurrence of <b>syslog authentication failures</b> observed on the deployed Linux server, which indicates unauthorized access attempts originating from external sources. This serves as a pivotal reminder of the indispensable need to prioritize the security of Linux servers by implementing robust authentication mechanisms and maintaining vigilant monitoring of system logs to promptly detect and respond to any signs of intrusion attempts.

![(Before) windows-rdp-smb-auth-fail](https://github.com/ryanjustindejesus/Azure-SOC-Honey-Project/blob/main/windows-rdp-auth-fail.png)

- This visual representation serves as a compelling reminder of the paramount importance of securing remote access and file sharing services. By implementing robust security measures in these areas, organizations can effectively mitigate the risk of unauthorized access and potential cyber threats, ensuring the protection of valuable data and resources.

By understanding the implications showcased in these attack maps, organizations can better appreciate the need for robust security measures to protect their networks, systems, and data from malicious actors.

## Metrics Before Hardening / Security Controls

- <b>The following table shows the metrics we measured in our insecure environment for 24 hours:</b>
- <b>Start Time   2024-10-01 08:48:43</b>
- <b>Stop Time    2024-10-02 08:48:43</b>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 14336
| Syslog                   | 1610
| SecurityAlert            | 0
| SecurityIncident         | 162
| AzureNetworkAnalytics_CL | 1386

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

- <b>The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:</b>
- <b>Start Time 2024-10-02 19:22:57</b>
- <b>Stop Time	2024-10-03 19:22:57</b>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 407
| Syslog                   | 3
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 48

## Conclusion

This project involved the construction of a mini honeynet within Microsoft Azure, integrating log sources into a Log Analytics workspace. Microsoft Sentinel was utilized to generate alerts and incidents based on the ingested logs. Metrics were measured in the initial insecure environment and later after implementing security measures. Notably, the implementation of security controls led to a significant reduction in the number of security events and incidents, highlighting their effectiveness.

Importantly, it should be acknowledged that if the network resources were extensively utilized by regular users, it is possible that a higher number of security events and alerts would have been generated within the 24-hour period following the implementation of the security controls.

# SOC Monitoring and Threat Detection Lab

## Project Overview
This project demonstrates the setup and use of a Security Information and Event Management (SIEM) environment using Wazuh to monitor Windows security events, detect suspicious authentication activity and investigate security alerts.

I deployed Wazuh in a Linux virtual machine, connected a Windows 11 endpoint using the Wazuh agent, and developed a custom correlation rule to detect three failed login attempts against the same account within 120 seconds.

I then simulated failed logins, verified the resulting alerts against Windows Security logs, and documented the investigation findings, including the classification of the activity and recommended response.

## Objectives
- Deploy a Wazuh SIEM environment using VirtualBox and configure a Windows 11 endpoint for security event monitoring.
- Develop and test a custom correlation rule to identify repeated failed Windows logins within a defined timeframe.
- Investigate generated alerts using Wazuh and Windows Security logs, document findings, and determine an appropriate response.

## Lab Architecture
The SOC lab was built on a Windows 11 Home laptop using Oracle VirtualBox. The Wazuh server runs inside a Linux virtual machine, while the Wazuh agent is installed directly on the Windows host.

| Component | Configuration |
|---|---|
| Host OS | Windows 11 Home, 16 GB RAM |
| Virtualisation | Oracle VirtualBox |
| Wazuh Server | Linux VM, ~6 GB RAM, 4 vCPUs |
| Monitoring | Wazuh Agent 4.14.8 on Windows |
| Host-only Network | Windows: 192.168.26.1; Wazuh: 192.168.26.100 |
| NAT Network | Outbound connectivity for Wazuh VM |

### Security Event Flow
1. Windows records authentication events.
2. The Wazuh agent collects configured security events and forwards them to the Wazuh Manager.
3. The Manager applies detection rules and generates alerts.
4. Filebeat forwards alert data to the Wazuh Indexer.
5. The Dashboard allows analysts to search and investigate alerts.

## Tools and Technologies
| Tool / Technology | Purpose |
|---|---|
| Oracle VirtualBox | Hosted the Wazuh Linux VM and configured virtual networking. |
| Wazuh 4.14.8 | Analysed security events and generated searchable alerts. |
| Wazuh Windows Agent | Collected Windows security logs and forwarded them to the manager. |
| Windows Event Viewer | Investigated authentication events 4624 and 4625. |
| PowerShell | Managed Windows services and queried security events. |
| Linux command line | Checked system health, managed Wazuh services and validated rules. |
| XML | Created the custom correlation detection rule. |
| Git and Markdown | Organised local project files and documented the investigation. |

## Implementation

### 1. Wazuh Server Setup

Imported the Wazuh 4.14.8 virtual appliance into Oracle VirtualBox on a Windows 11 Home laptop.

Configured the VM with approximately 6 GB RAM, four virtual CPUs and a 25 GB virtual disk.

Configured two virtual network adapters:

- **NAT Network:** Provided outbound network connectivity.
- **Host-only Adapter:** Enabled communication between the Windows host (`192.168.26.1`) and the Wazuh server (`192.168.26.100`).

Verified the VM's IP addresses using `ip -br -4 addr`, checked disk availability using `df -h`, and confirmed that the Wazuh Manager, Indexer and Dashboard services were running.

### 2. Windows Agent Deployment

Installed Wazuh Agent 4.14.8 directly on the Windows 11 host and configured it to communicate with the Wazuh Manager at `192.168.26.100`.

Registered the endpoint as `win11-lab-host` and verified its Active status in the Wazuh Dashboard.

Confirmed network connectivity using TCP port 1515 for initial enrolment and TCP port 1514 for regular agent communication.

Verified that the agent was configured to collect Windows Security events through the `eventchannel` log format.

Finally, confirmed that Windows Event IDs `4624` and `4625` produced searchable alerts in Wazuh.

## Detection Rule

## Testing and Investigation

## Results and Limitations

## Lessons Learned
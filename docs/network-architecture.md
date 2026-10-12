\# Network Architecture



\## Overview

This lab simulates a real-world enterprise environment using an isolated virtual network, completely separated from the home LAN, to safely run offensive security tools and generate detectable attack telemetry.



\## Network Design

\- \*\*Network Type:\*\* VirtualBox NAT Network (isolated, not bridged to host LAN)

\- \*\*Subnet:\*\* 10.10.10.0/24

\- \*\*Internet Access:\*\* Yes (for package/tool downloads), but no inbound access from external networks



\## Hosts



| Hostname | Role | IP Address | OS |

|---|---|---|---|

| kali-linux | Attacker | 10.10.10.x | Kali Linux 2026.2 |

| ubuntu-victim | Target | 10.10.10.5 | Ubuntu Server 24.04 LTS |

| splunk-server | SIEM | 10.10.10.6 | Ubuntu Server 24.04 LTS |



\## Design Decisions

\- Used a NAT Network instead of Host-Only or Bridged networking to allow VM-to-VM communication and internet access for package installation, while keeping the lab fully isolated from the home network.

\- Host-to-VM access (SSH, Splunk web UI) required explicit port-forwarding rules on the NAT Network, since NAT Networks do not expose VMs directly to the host by default.

\- All VM builds were done via VirtualBox's command-line tool (VBoxManage) rather than the GUI, to practice infrastructure-as-code style provisioning and ensure repeatability.



\## Port Forwarding Rules

| Rule Name | Protocol | Host Port | Destination VM | Destination Port |

|---|---|---|---|---|

| ssh-splunk | TCP | 2222 | 10.10.10.6 | 22 |

| splunkweb | TCP | 8000 | 10.10.10.6 | 8000 |


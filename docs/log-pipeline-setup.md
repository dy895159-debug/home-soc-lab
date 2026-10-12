Markdown



\# Log Pipeline Setup: Ubuntu-Victim → Splunk



\## Overview

This document covers the end-to-end data pipeline that collects security-relevant Linux audit events from the victim machine and forwards them to the central Splunk instance for indexing, search, and future detection rule development.



\## Components

\- \*\*auditd\*\* — Linux kernel-level audit framework running on Ubuntu-Victim, generating logs for process execution, file access, authentication events, and privilege use.

\- \*\*Splunk Universal Forwarder\*\* — lightweight agent installed on Ubuntu-Victim that monitors `/var/log/audit/audit.log` and forwards new events to Splunk-Server in real time.

\- \*\*Splunk Enterprise\*\* — running on Splunk-Server, configured to receive forwarder traffic on TCP port 9997 and index incoming events for search.



\## Pipeline Flow

auditd (generates logs on Ubuntu-Victim)

\-> /var/log/audit/audit.log

\-> Splunk Universal Forwarder (monitors file, ships over TCP 9997)

\-> Splunk-Server (listening on 9997, indexes events)

\-> Searchable in Splunk Web UI



text





\## Configuration Steps

1\. Installed `auditd` and `audispd-plugins` on Ubuntu-Victim via apt.

2\. Installed Splunk Universal Forwarder on Ubuntu-Victim.

3\. Configured forwarder to send data to Splunk-Server: `splunk add forward-server 10.10.10.6:9997`

4\. Enabled receiving on Splunk-Server: `splunk enable listen 9997`

5\. Added a monitor stanza on the forwarder for the audit log: `splunk add monitor /var/log/audit/audit.log`



\## Verification

Confirmed successful ingestion via Splunk search:

index=\* host=ubuntu-victim



text



Result: 969 events ingested within the first 24-hour window, confirming the full pipeline (auditd -> forwarder -> Splunk-Server -> indexed/searchable) is operational.



!\[Splunk ingesting auditd logs](../screenshots/splunk-ingesting-auditd-logs.png)



\## Networking Note

Since all VMs run on an isolated VirtualBox NAT Network (10.10.10.0/24), inter-VM communication (forwarder-to-indexer traffic on port 9997) works natively without additional configuration. Host-to-VM access (e.g., SSH, Splunk Web UI from the Windows host) required explicit port-forwarding rules, documented in `network-architecture.md`.


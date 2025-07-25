# Threat-Intel-Pipeline-Sentinel
This project simulates a proactive threat detection pipeline using Microsoft Sentinel. It focuses on ingesting domain-based threat intelligence, correlating it with DNS logs, and triggering automated incident response via Logic Apps.

 Objectives
1.Ingest domain IOCs into Microsoft Sentinel using Watchlists
2.Detect DNS resolution to known malicious domains
3.Trigger scheduled analytics rule and generate incidents
4.Automate response using a Logic App (Email alert)

```📁 Folder Structure
Threat-Intel-Pipeline-Sentinel/
├── watchlist/
│   └── threat-iocs-domains.csv                        # Domain IOCs to be ingested as watchlist
├── kql/
│   └── dns-iocs.kql                 # Final detection query to correlate DNS logs
├── logicapp/
│   └── alert-notify-playbook.json             # Logic App export that triggers upon incident
├── docs/
│   └── hunting-guide.md                       # Step-by-step documentation of the workflow
│   └── screenshots/                           # Screenshots of rule setup, logs, and alerting
├── README.md                                  # Project overview (this file)
```
Tools Used
```
Tool	                           Purpose
Microsoft Sentinel	             SIEM platform for log collection, correlation, and detection
Sentinel Watchlists	             Uploading and referencing domain IOCs
KQL (Kusto Query Language)	     Writing detection logic against DNS logs
Logic Apps	                     Automating alerts (e.g., sending emails) upon incident
Windows VM	                     Simulated endpoint for generating suspicious DNS queries
```
Results
1.IOC-based correlation using domain watchlists successfully triggered incidents
2.Logic App sent email alerts automatically
3.All activity logged and visualized in Sentinel
4.Project fully documented and reproducible

How to Use
1.Upload your list of domain IOCs to the watchlist/ folder
2.Deploy the detection KQL in Sentinel Analytics
3.Trigger the detection by simulating DNS requests
4.Attach your Logic App playbook to respond automatically

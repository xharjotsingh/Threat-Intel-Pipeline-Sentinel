# Threat Intel Hunting Guide

## Project Goal

Build a threat intelligence detection pipeline that ingests known malicious domain indicators from external CTI feeds, correlates them with DNS logs in Microsoft Sentinel, and triggers automated alerts when matches are found.

---

## IOC Sources

The following domains were sourced from external threat intelligence feeds and uploaded as a watchlist:

- `0-u2c.xyz`
- `1-mail.ga`
- `2020adblocker.com`
- `2captcha-billing.com`
- `2captcha-verify.info`
- ... and more.

These were saved in a CSV named: `threat-iocs-domains.csv` with the column header `MaliciousDomains`.

---

## Watchlist Upload Process

1. Go to: **Microsoft Sentinel > Configuration > Watchlist**
2. Click **+ Add new**
3. Set the name as: `ThreatIntelFeed`
4. Upload the file: `threat-iocs-domains.csv`
5. Set **Alias** to: `ThreatIntelFeed`
6. Finish wizard and validate with:
   ```kql
   _GetWatchlist('ThreatIntelFeed')
   ```

---

## Detection Rule (KQL)

This KQL query checks for any DNS resolution of domains present in the threat watchlist:

```kql
let BadDomains = _GetWatchlist('ThreatIntelFeed') | project MaliciousDomains; 
Event 
| where EventLog == "Microsoft-Windows-DNS-Client/Operational" 
| where RenderedDescription has_any (BadDomains) 
| project TimeGenerated, Computer, EventLog, RenderedDescription

```

This was used to correlate DNS client logs with the domain IOC list.

---

## 🛡️ Detection Rule Setup

1. Go to: **Sentinel > Analytics > + Create > Scheduled Query Rule**
2. Rule Name: `Threat Intel Correlation - DNS`
3. Frequency: Every 5 minutes
4. Lookup Period: Last 5 minutes
5. Incident Creation: ✅ Enabled
6. Severity: High
7. Entity Mapping:
   - `Computer` → Host
---

## Automated Response with Logic App

1. Created a Logic App named: `alert-notify-playbook`
2. Trigger: **When Sentinel incident is created**
3. Action: **Send email to @ ** with incident details
4. Assigned the role to the Logic App at the resource group level

---

## IOC Simulation and Testing

1. Ran the following DNS lookups from the monitored VM:
   ```powershell
   nslookup 0-u2c.xyz
   nslookup 1-mail.ga
   nslookup 2020adblocker.com
   ```
2. Waited ~5 minutes for logs to ingest
3. Confirmed the domain appeared in DNS logs via KQL query
4. Detection rule matched the IOC → Incident was created → Logic App triggered
---

## Outcome

| Component | Purpose |
|----------|---------|
| `threat-iocs-domains.csv` | IOC source |
| Watchlist | Stores domain indicators |
| KQL | Detects DNS activity |
| Analytics Rule | Triggers incident |
| Logic App | Sends Email |
| Screenshots & Guide | Documentation for replication |


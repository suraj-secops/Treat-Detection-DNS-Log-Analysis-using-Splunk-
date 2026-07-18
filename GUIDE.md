📊 DNS Traffic Analysis & Threat Detection Using Splunk


  Analyzing Zeek-style DNS logs with Splunk Enterprise for SOC investigations


---

## 📌 Project Overview

This project demonstrates how Zeek-style DNS logs can be analyzed in Splunk Enterprise to investigate DNS activity from a SOC analyst's perspective. DNS events are ingested in JSON format and explored using Splunk Search Processing Language (SPL) to identify frequently queried domains, active client systems, DNS record usage, and indicators of suspicious DNS behavior.

---

## 🛠️ Tools & Technologies

* **Splunk Enterprise**
* **Zeek DNS Logs (JSON)**
* **Splunk Search Processing Language (SPL)**

---

## 📂 Dataset Information

The lab uses Zeek-style DNS events stored in JSON format. Each event contains metadata required to investigate DNS traffic, including:

```text
ts          → Event timestamp
id.orig_h   → Client IP address
id.resp_h   → DNS server IP
qtype       → DNS record type
query       → Requested domain
answers     → DNS response
rcode       → DNS response code
rtt         → DNS round-trip time
```

---

## ⚙️ Lab Setup & Data Ingestion

### Step 1 – Upload DNS Logs

1. Open **Splunk Web**.
2. Navigate to:

```text
Settings → Add Data → Upload
```

3. Select:

```text
dns_logs.json
```

4. Configure the data source:

* **Source Type:** `json` (or custom `zeek:dns`)
* **Index:** `dns_lab`

---

### Step 2 – Verify Data Ingestion

Run the following query to confirm that events have been indexed successfully.

```spl
index=dns_lab | head 5
```

---

# 🔍 DNS Investigation Queries

## 🔹 Task 1 – Most Frequently Queried Domains

Identify the domains receiving the highest number of DNS requests.

```spl
index=dns_lab
| stats count by query
| sort -count
```

**Security Relevance**

Repeated requests to unexpected or uncommon domains may indicate malware beaconing, command-and-control communication, or unauthorized applications.

---

## 🔹 Task 2 – Most Active Client Systems

Identify hosts generating the highest DNS request volume.

```spl
index=dns_lab
| stats count by id.orig_h
| sort -count
```

**Security Relevance**

A client producing significantly higher DNS traffic than other systems may require investigation for compromise, automated activity, or configuration issues.

---

## 🔹 Task 3 – DNS Record Type Distribution

Analyze the distribution of DNS record types observed in the environment.

```spl
index=dns_lab
| stats count by qtype
```

Common DNS record types:

* `A` → IPv4 Address
* `AAAA` → IPv6 Address
* `CNAME` → Canonical Name
* `PTR` → Reverse DNS Lookup

**Security Relevance**

Understanding normal DNS record usage helps establish a baseline and makes unusual query patterns easier to identify.

---

## 📈 Analysis Summary

The analysis revealed the most frequently requested domains, identified hosts generating the highest DNS traffic, and provided insight into DNS record type usage. DNS response information and query patterns helped establish a baseline for normal DNS activity within the dataset.

---

## 🚨 Security Insights

DNS telemetry can support investigations involving:

* Malware beaconing
* DNS tunneling
* Excessive DNS request volume
* Failed DNS lookups
* Rare or newly observed domains
* Reverse DNS lookup anomalies

---

## 📊 Future Improvements

Planned enhancements include:

* Interactive Splunk dashboards
* Alerts for unusual DNS activity
* NXDOMAIN monitoring
* Detection of rare domain queries
* Correlation with HTTP, Proxy, or Firewall logs
* GeoIP enrichment for DNS responses

---

## ✅ Conclusion

This project demonstrates how DNS logs can be ingested, searched, and investigated using Splunk Enterprise. The SPL queries developed throughout the lab provide practical experience in DNS traffic analysis and help build foundational SIEM skills relevant to SOC Analyst and Blue Team roles.

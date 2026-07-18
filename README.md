# DNS Log Analysis Using Splunk (Zeek Logs)

**SOC lab demonstrating DNS log ingestion, investigation, and threat detection using Splunk Enterprise.**

---

## 📌 Overview

This project showcases the process of collecting, indexing, and analyzing Zeek-style DNS logs in Splunk Enterprise. Using Splunk Search Processing Language (SPL), DNS events are examined to understand normal network behavior and identify indicators of suspicious activity commonly investigated by SOC analysts.

The analysis focuses on:

* Frequently requested domain names
* Systems generating excessive DNS traffic
* Distribution of DNS record types
* Detection of unusual DNS activity

---

## 🎯 Objectives

This lab was designed to provide practical experience with:

* Importing JSON-based DNS logs into Splunk
* Parsing key DNS fields for analysis
* Writing SPL searches to investigate DNS traffic
* Identifying abnormal DNS patterns
* Understanding how DNS telemetry supports SOC investigations

---

## 🛠️ Technologies Used

* Splunk Enterprise
* Zeek DNS Logs (JSON)
* Splunk Search Processing Language (SPL)

---

## 📂 Dataset

The dataset contains DNS events generated in Zeek JSON format. Each event includes fields such as:

* `ts` – Event timestamp
* `id.orig_h` – Client IP address
* `id.resp_h` – DNS server IP
* `query` – Requested domain
* `qtype` – DNS record type
* `answers` – DNS response
* `rcode` – Response status
* `rtt` – Query round-trip time

---

## ⚙️ Data Ingestion

### Step 1 – Import DNS Logs

In Splunk Web:

**Settings → Add Data → Upload**

Upload:

`dns_logs.json`

Recommended configuration:

* Source Type: `json` (or custom `zeek:dns`)
* Index: `dns_lab`

### Step 2 – Validate Ingestion

Confirm that events have been indexed successfully.

```spl
index=dns_lab | head 5
```

---

# DNS Analysis

## 1. Top Queried Domains

Identify the domains receiving the highest number of DNS requests.

```spl
index=dns_lab
| stats count by query
| sort -count
```

**Why it matters**

Frequently queried domains may represent legitimate business services, while unexpected or repetitive requests can indicate malware communication or command-and-control activity.

---

## 2. Most Active DNS Clients

Determine which hosts generate the largest volume of DNS requests.

```spl
index=dns_lab
| stats count by id.orig_h
| sort -count
```

**Why it matters**

A host producing unusually high DNS traffic could indicate automated processes, misconfiguration, or potential compromise.

---

## 3. DNS Record Type Analysis

Review the distribution of DNS record types observed in the environment.

```spl
index=dns_lab
| stats count by qtype
```

Common record types include:

* **A** – IPv4 Address
* **AAAA** – IPv6 Address
* **CNAME** – Alias Record
* **PTR** – Reverse DNS Lookup

Understanding record distribution helps establish a baseline for normal DNS activity.

---

## 📈 Results

The analysis enabled:

* Identification of the most frequently requested domains
* Detection of clients generating the highest DNS traffic
* Review of DNS record type distribution
* Observation of DNS response times and failed lookups
* Establishment of baseline DNS behavior for the environment

---

## 🚨 Security Value

DNS analysis plays an important role in detecting:

* DNS tunneling attempts
* Malware beaconing
* Excessive reverse DNS lookups
* Large numbers of failed DNS requests
* Unusual or previously unseen domains

---

## ✅ Summary

This project provided hands-on experience with DNS log analysis in Splunk Enterprise. It involved ingesting Zeek-style logs, developing SPL searches, investigating DNS activity, and identifying behaviors relevant to SOC monitoring and threat detection. The lab demonstrates practical SIEM skills applicable to SOC Analyst and Blue Team roles.
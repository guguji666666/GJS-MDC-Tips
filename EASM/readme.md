
# 🧩 EASM API Guide

EASM (External Attack Surface Management) provides API access to query asset data such as domains, IPs, and host\:IP pairs. This guide outlines how to extract **billable assets** using browser-based tracing and offers sample output formats.

---

## 📚 Table of Contents

1. [Understanding Billable Assets](#1-understanding-billable-assets)
2. [Steps to Query via EASM API](#2-steps-to-query-via-easm-api)
   2.1. [Access the Azure Portal](#21-access-the-azure-portal)
   2.2. [Navigate to EASM Inventory](#22-navigate-to-easm-inventory)
   2.3. [Customize the Query](#23-customize-the-query)
   2.4. [Trace API Calls Using Browser Tools](#24-trace-api-calls-using-browser-tools)
   2.5. [Capture the HAR File](#25-capture-the-har-file)
3. [Sample Outputs](#3-sample-outputs)

---

## 1. Understanding Billable Assets

As described in the official documentation:
👉 [Understand billable assets - Microsoft Learn](https://learn.microsoft.com/en-us/defender-for-cloud/external-attack-surface-management/understand-billable-assets)

The following assets are considered **billable**:

* ✅ Approved **host\:IP combinations**
* ✅ Approved **domains**
* ✅ Approved **IP addresses**

> ⚠️ **Only assets in the "Approved Inventory" state are billable.**
> Assets in other states are not counted, and duplicate host entries are **de-duplicated** automatically.

So, billable assets include **more than just** domains and IPs — they also account for host\:IP pairs that have been **observed and approved**.

---

## 2. Steps to Query via EASM API

### 2.1. Access the Azure Portal

Go to: [https://portal.azure.com](https://portal.azure.com)

---

### 2.2. Navigate to Microsoft Defender EASM

`Microsoft Defender EASM` → `General` → `Inventory`

---

### 2.3. Customize the Query

Use the filter options to narrow down the inventory. Below is a sample:

![Sample Query Screenshot](https://github.com/user-attachments/assets/b02fbcdd-23f9-4b79-82ca-2f96feccc61e)

---

### 2.4. Trace API Calls Using Browser Tools

1. Press `F12` or open your browser’s **Developer Tools**
2. Go to the **Network** tab
3. Perform a search in the UI
4. Observe the endpoint being called
5. Copy the token used in the request header

---

### 2.5. Capture the HAR File

Export the HAR file to analyze or replicate API requests.

![HAR Trace Example](https://github.com/user-attachments/assets/094758e7-6c04-4b5d-837e-95639b60f1f1)

---

## 3. Sample Outputs

Below are sample responses returned by the API for different asset types (expandable in the EASM portal):

---

### 🔹 Host Assets

```json
{
  "id": "host$$ssor.gjnsw.webproxy.id.hao123.com",
  "name": "ssor.gjnsw.webproxy.id.hao123.com",
  "displayName": "ssor.gjnsw.webproxy.id.hao123.com",
  "kind": "host",
  "uuid": "b227925d-083f-fa2b-0983-90303d84495d",
  "asset": {
    "host": "ssor.gjnsw.webproxy.id.hao123.com",
    "domain": "hao123.com",
    "ipAddresses": [
      {
        "value": "108.160.161.20",
        "firstSeen": "2025-06-05T23:58:12Z",
        "lastSeen": "2025-06-05T23:58:17Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "199.59.148.102",
        "firstSeen": "2025-06-05T15:01:11Z",
        "lastSeen": "2025-06-05T15:01:11Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "108.160.167.147",
        "firstSeen": "2025-06-05T00:07:30Z",
        "lastSeen": "2025-06-05T00:07:35Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "174.36.228.136",
        "firstSeen": "2025-06-04T14:47:56Z",
        "lastSeen": "2025-06-04T14:47:56Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "208.101.60.87",
        "firstSeen": "2025-06-03T18:30:23Z",
        "lastSeen": "2025-06-03T18:30:28Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "74.86.228.110",
        "firstSeen": "2025-04-27T17:45:19Z",
        "lastSeen": "2025-06-03T01:15:52Z",
        "count": 4,
        "sources": [],
        "recent": true
      }
    ]
  }
}
```

---

### 🔹 Domain Assets

```json
{
  "id": "host$$ssor.gjnsw.webproxy.id.hao123.com",
  "name": "ssor.gjnsw.webproxy.id.hao123.com",
  "displayName": "ssor.gjnsw.webproxy.id.hao123.com",
  "kind": "host",
  "uuid": "b227925d-083f-fa2b-0983-90303d84495d",
  "asset": {
    "host": "ssor.gjnsw.webproxy.id.hao123.com",
    "domain": "hao123.com",
    "ipAddresses": [
      {
        "value": "108.160.161.20",
        "firstSeen": "2025-06-05T23:58:12Z",
        "lastSeen": "2025-06-05T23:58:17Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "199.59.148.102",
        "firstSeen": "2025-06-05T15:01:11Z",
        "lastSeen": "2025-06-05T15:01:11Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "108.160.167.147",
        "firstSeen": "2025-06-05T00:07:30Z",
        "lastSeen": "2025-06-05T00:07:35Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "174.36.228.136",
        "firstSeen": "2025-06-04T14:47:56Z",
        "lastSeen": "2025-06-04T14:47:56Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "208.101.60.87",
        "firstSeen": "2025-06-03T18:30:23Z",
        "lastSeen": "2025-06-03T18:30:28Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "74.86.228.110",
        "firstSeen": "2025-04-27T17:45:19Z",
        "lastSeen": "2025-06-03T01:15:52Z",
        "count": 4,
        "sources": [],
        "recent": true
      }
    ]
  }
}
```

---

### 🔹 IP Address Assets

```json
{
  "id": "host$$ssor.gjnsw.webproxy.id.hao123.com",
  "name": "ssor.gjnsw.webproxy.id.hao123.com",
  "displayName": "ssor.gjnsw.webproxy.id.hao123.com",
  "kind": "host",
  "uuid": "b227925d-083f-fa2b-0983-90303d84495d",
  "asset": {
    "host": "ssor.gjnsw.webproxy.id.hao123.com",
    "domain": "hao123.com",
    "ipAddresses": [
      {
        "value": "108.160.161.20",
        "firstSeen": "2025-06-05T23:58:12Z",
        "lastSeen": "2025-06-05T23:58:17Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "199.59.148.102",
        "firstSeen": "2025-06-05T15:01:11Z",
        "lastSeen": "2025-06-05T15:01:11Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "108.160.167.147",
        "firstSeen": "2025-06-05T00:07:30Z",
        "lastSeen": "2025-06-05T00:07:35Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "174.36.228.136",
        "firstSeen": "2025-06-04T14:47:56Z",
        "lastSeen": "2025-06-04T14:47:56Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "208.101.60.87",
        "firstSeen": "2025-06-03T18:30:23Z",
        "lastSeen": "2025-06-03T18:30:28Z",
        "count": 2,
        "sources": [],
        "recent": true
      },
      {
        "value": "74.86.228.110",
        "firstSeen": "2025-04-27T17:45:19Z",
        "lastSeen": "2025-06-03T01:15:52Z",
        "count": 4,
        "sources": [],
        "recent": true
      }
    ]
  }
}

```



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
  "id": "domain$$nic.baidu",
  "name": "nic.baidu",
  "displayName": "nic.baidu",
  "kind": "domain",
  "uuid": "3db519a9-26c3-0eee-0252-6fbf0ef1ba85",
  "asset": {
    "domain": "nic.baidu",
    "whoisId": 6796577505194440547,
    "registrarIanaIds": [
      {
        "value": 9999,
        "firstSeen": "2016-12-16T17:11:18Z",
        "lastSeen": "2025-06-02T04:22:04Z",
        "sources": [],
        "recent": true
      }
    ],
    "registrantContacts": [
      {
        "value": "domainmaster@baidu.com",
        "firstSeen": "2018-10-18T13:16:51Z",
        "lastSeen": "2025-06-02T04:22:04Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "zhangli@cnnic.cn",
        "firstSeen": "2016-12-16T17:11:18Z",
        "lastSeen": "2016-09-24T00:00:00Z",
        "sources": []
      }
    ],
    "registrantOrgs": [
      {
        "value": "百度在线网络技术（北京）有限公司",
        "firstSeen": "2018-10-18T13:16:51Z",
        "lastSeen": "2025-06-02T04:22:04Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "Ã§Â¾Ã¥ÂºÃ¥Â¨Ã§ÂºÂ¿Ã§Â½Ã§Â»ÃÃÂ¯Ã¯Â¼Ã¥Ã¤ÂºÂ¬Ã¯Â¼ÃÃÃ¥Â¬Ã¥Â¸",
        "firstSeen": "2021-03-05T17:19:11Z",
        "lastSeen": "2021-05-07T23:32:31Z",
        "sources": []
      },
      {
        "value": "ç¾åºå¨çº¿ç½ç»ææ¯ï¼åäº¬ï¼æéå¬å¸",
        "firstSeen": "2020-09-04T12:06:25Z",
        "lastSeen": "2020-09-04T12:06:25Z",
        "sources": []
      },
      {
        "value": "baidu, inc.",
        "firstSeen": "2016-12-16T17:11:18Z",
        "lastSeen": "2016-09-24T00:00:00Z",
        "sources": []
      }
    ],
    // ... 以下内容省略以便节省空间，内容结构一致，如需全部保留请说明
  },
  "createdDate": "2025-03-27T04:17:37Z",
  "updatedDate": "2025-03-27T04:17:37Z",
  "state": "confirmed",
  "externalId": null,
  "labels": [],
  "wildcard": false,
  "discoGroupName": null,
  "auditTrail": [
    {
      "id": null,
      "name": "baidu.com",
      "displayName": null,
      "kind": "domain",
      "reason": null
    },
    {
      "id": null,
      "name": "domainmaster@baidu.com",
      "displayName": null,
      "kind": "contact",
      "reason": null
    }
  ],
  "history": null,
  "reason": null
}
```

---

### 🔹 IP Address Assets

```json
{
  "id": "ipAddress$$2620:119:50c0:207:0:0:0:e8",
  "name": "2620:119:50c0:207:0:0:0:e8",
  "displayName": "2620:119:50c0:207:0:0:0:e8",
  "kind": "ipAddress",
  "uuid": "375e3fbc-981e-3e30-1b1a-1958700cd3ba",
  "asset": {
    "ipAddress": "2620:119:50c0:207:0:0:0:e8",
    "asns": [],
    "reputations": [],
    "webComponents": [],
    "netRanges": [],
    "headers": [],
    "attributes": [],
    "cookies": [],
    "sslCerts": [],
    "services": [],
    "ipBlocks": [],
    "sources": [],
    "firstSeen": "2020-11-26T05:28:10Z",
    "lastSeen": "2025-06-05T16:43:19Z",
    "count": 867,
    "banners": [],
    "scanMetadata": [],
    "nsRecord": [
      {
        "value": false,
        "firstSeen": "2020-11-26T05:28:10Z",
        "lastSeen": "2025-06-05T16:43:19Z",
        "sources": [],
        "recent": true
      }
    ],
    "mxRecord": [
      {
        "value": false,
        "firstSeen": "2020-11-26T05:28:10Z",
        "lastSeen": "2025-06-05T16:43:19Z",
        "sources": [],
        "recent": true
      }
    ],
    "location": [
      {
        "value": {
          "countrycode": "US",
          "countryname": "United States",
          "latitude": 37.751,
          "longitude": -97.822
        },
        "firstSeen": "2020-11-26T05:28:10Z",
        "lastSeen": "2025-06-05T16:43:19Z",
        "recent": true,
        "sources": []
      },
      {
        "value": {
          "countrycode": "US",
          "countryname": "United States",
          "region": "CA",
          "regionname": "California",
          "city": "San Jose",
          "postalcode": "95117",
          "latitude": 37.3083,
          "longitude": -121.9643,
          "metrocodeid": 807
        },
        "firstSeen": "2021-06-15T08:36:58Z",
        "lastSeen": "2021-07-01T13:17:34Z",
        "sources": []
      },
      {
        "value": {
          "countrycode": "US",
          "countryname": "United States",
          "region": "CA",
          "regionname": "San Francisco",
          "postalcode": "94117",
          "latitude": 37.7703,
          "longitude": -122.4407,
          "metrocodeid": 807
        },
        "firstSeen": "2021-01-13T17:17:46Z",
        "lastSeen": "2021-01-17T15:42:16Z",
        "sources": []
      }
    ],
    "hosts": [
      {
        "value": "mail732.linkedin.com",
        "firstSeen": "2020-12-21T17:17:48Z",
        "lastSeen": "2025-06-05T16:43:19Z",
        "sources": [],
        "recent": true
      },
      {
        "value": "hitlist-rr6.hitlist.sdstrowes.co.uk",
        "firstSeen": "2022-02-14T18:24:49Z",
        "lastSeen": "2023-11-20T17:46:45Z",
        "sources": []
      }
    ],
    "nxdomain": [],
    "sslServerConfig": [],
    "ipv4": false,
    "ipv6": true
  },
  "createdDate": "2025-03-27T05:22:13Z",
  "updatedDate": "2025-03-27T05:22:13Z",
  "state": "confirmed",
  "externalId": null,
  "labels": [],
  "wildcard": false,
  "discoGroupName": null,
  "auditTrail": [
    {
      "id": null,
      "name": "linkedin.com",
      "displayName": null,
      "kind": "domain",
      "reason": null
    },
    {
      "id": null,
      "name": "mail732.linkedin.com",
      "displayName": null,
      "kind": "host",
      "reason": null
    }
  ],
  "history": null,
  "reason": null
}
```


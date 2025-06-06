# EASM API

## 1. Get billed assets (partially)

As documented in our official guidance (Understand billable assets - Microsoft Learn), the following types of assets are considered billable:

Approved host:IP combinations

Approved domains

Approved IP addresses

Please note that only assets in the Approved Inventory state are counted as billable. No charges are incurred for assets in other states. In addition, duplicate host assets are de-duplicated and not included in the final billable count.

This means that the billable asset count is not limited to just approved domains and IP addresses—it also includes host:IP combinations that have been observed and approved

## Guidance to Call API
### 1. navigate to azure portal - portal.azure.com
### 2. navigate to Microsoft Defender EASM > Gerneal > Inventory
### 3. customize the query here (berlow is the sample)
![image](https://github.com/user-attachments/assets/b02fbcdd-23f9-4b79-82ca-2f96feccc61e)
### 4. press F12 or manually open developer tool in browser to get netwqork track so that we could check endpoint being called and token used in header
### 5. Run the search and capture the har tracing
![image](https://github.com/user-attachments/assets/094758e7-6c04-4b5d-837e-95639b60f1f1)




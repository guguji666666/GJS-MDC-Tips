# EASM API

## 1. Get billed assets (partially)

As documented in our official guidance (Understand billable assets - Microsoft Learn), the following types of assets are considered billable:

Approved host:IP combinations

Approved domains

Approved IP addresses

Please note that only assets in the Approved Inventory state are counted as billable. No charges are incurred for assets in other states. In addition, duplicate host assets are de-duplicated and not included in the final billable count.

This means that the billable asset count is not limited to just approved domains and IP addresses—it also includes host:IP combinations that have been observed and approved

### Guidance to Call API

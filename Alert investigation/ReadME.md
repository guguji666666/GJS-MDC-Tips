# MDC alerts investigation

## 1. OMS runs apt managed scripts - adaptive application control alert

### Reason : If the solution "securityCenterFree" has been pushed to the VM, then the behavior is expected

### ARG query to verify the `solutions` pushed to the VM
```kusto
Heartbeat
| where Computer contains "<name of the VM>"
| project TimeGenerated, Computer, ResourceId, Solutions
```

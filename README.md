# Onboarding WEF

Windows offers a native functionality to forward the logs to central location (Collector).
There are two ways to onboard:

 - Collector Initiated: Simple, not scalable, only worth up to 5 devices to forward logs.
 - Source Initiated: Extra steps to onboard, very scalable, setup using a GPO.
 
 
 ## Source Initiated Log Forwarding
 ### Steps in the Forwarder
 
In the Forwarder, check the Security Event Log Security Descriptor

```powershell
wevtutil gl security
```


### Steps in a Domain Controller 
Configure a GPO "WEF".
 Items to configure in the GPO
- **Computer Configuration --> Policies --> Administrative Templates --> Windows Components --> Event Forwarding --> Configure target Subscription Manager**
```
Server=http://<collector>:5985/wsman/SubscriptionManager/WEC,Refresh=60
```
- **Computer Configuration --> Policies --> Administrative Templates --> Windows Components --> Event Forwarding --> Configure forwarder resource usage**
```
events/sec = 100
```
- **Computer Configuration --> Policies --> Administrative Templates --> Windows Components --> Event Log Service --> Security**
```
Add network service SID to Log Access in Security Logs
Log Access {...}(A;;0x1;;;s-1-5-20)
```

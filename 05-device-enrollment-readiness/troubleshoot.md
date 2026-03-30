# Troubleshooting - Windows Time Service Issue

## Issue

During device readiness validation, the Windows Time service did not return status information.

Command used:

```cmd
w32tm /query /status
```

Error:

```txt
The following error occurred: The service has not been started. (0x80070426)
```

![Time service error](../assets/05-time-error.png)

---

## Investigation

### Check service state

```cmd
sc query w32time
```

Result:
- Service exists
- State: STOPPED

![Service query](../assets/05-time-service-query.png)

### Check service configuration

```cmd
sc qc w32time
```

Result:
- START_TYPE : 3 (DEMAND_START)
- Service set to manual start

![Service config](../assets/05-time-service-config.png)

---

## Resolution Step 1 - Set startup type

```cmd
sc config w32time start= auto
```

Result:
- Configuration updated successfully

![Service config fixed](../assets/05-time-service-config-fixed.png)

---

## Resolution Step 2 - Start service

```cmd
net start w32time
```

Result:
- Service started successfully

![Service start](../assets/05-time-service-start.png)

---

## Additional Issue - No Time Data

After starting the service, another issue appeared.

```cmd
w32tm /resync
```

Error:

```txt
The computer did not resync because no time data was available.
```

![Resync error](../assets/05-time-resync.png)

This indicates that no NTP source was configured.

---

## Resolution Step 3 - Configure NTP source

```cmd
w32tm /config /manualpeerlist:"time.windows.com" /syncfromflags:manual /update
```

Result:
- Command completed successfully

![NTP config](../assets/05-time-ntp-config.png)

---

## Resolution Step 4 - Restart service

```cmd
net stop w32time
net start w32time
```

Result:
- Service restarted successfully

![Service restart](../assets/05-time-service-restart.png)

---

## Resolution Step 5 - Retry synchronization

```cmd
w32tm /resync
```

Result:
- Synchronization successful

![Resync fixed](../assets/05-time-resync-fixed.png)

---

## Validation

```cmd
w32tm /query /status
```

Result:
- Source: time.windows.com
- Last successful sync time present
- No errors

![Time success](../assets/05-time-success.png)

---

## Root Cause

- Windows Time service was not running
- Startup type was set to manual
- No NTP source was configured

---

## Impact

If unresolved, this issue could affect:

- Time synchronization
- Authentication
- Certificate validation
- Microsoft Entra ID join
- MDM enrollment

---

## Conclusion

The issue was resolved by:

- setting the service to automatic
- starting the service
- configuring an NTP source
- restarting the service
- validating successful synchronization

The device is now ready for further enrollment scenarios.

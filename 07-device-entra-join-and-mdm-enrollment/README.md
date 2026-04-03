# Lab 07 – Microsoft Entra ID Join and Intune MDM Enrollment

## Objective

Join a Windows 11 device to Microsoft Entra ID and validate automatic enrollment into Microsoft Intune (MDM).

---

## Environment

* Device: md102-client-01
* OS: Windows 11
* User: [admin@emd102labs.onmicrosoft.com](mailto:admin@emd102labs.onmicrosoft.com)
* Tenant: IT LABS

---

## Step 1 – Join Device to Microsoft Entra ID

Navigate to:

Settings → Accounts → Access work or school

Click: Connect

Then select: Join this device to Microsoft Entra ID

Sign in with:

[admin@emd102labs.onmicrosoft.com](mailto:admin@emd102labs.onmicrosoft.com)

---

## Step 2 – Verify Join Status

Run:

dsregcmd /status

### Expected Results

* AzureAdJoined : YES
* DeviceAuthStatus : SUCCESS
* MdmUrl : Populated

### Evidence

![dsregcmd verification](assets/lab07-dsregcmd-status.png)

---

## Step 3 – Verify in Microsoft Entra Admin Center

Navigate to:

Entra ID → Devices → All devices

### Expected Results

* Device appears in the list
* Join type: Microsoft Entra joined

### Evidence

![entra device list](assets/lab07-entra-device-list.png)

---

## Step 4 – Verify in Microsoft Intune

Navigate to:

Intune Admin Center → Devices → All devices

### Expected Results

* Device is listed
* Managed by: Intune
* Compliance: Compliant

### Evidence

![intune device](assets/lab07-intune-device.png)

---

## Step 5 – Verify on Windows Device

Navigate to:

Settings → Accounts → Access work or school

### Expected Results

* Connected to Microsoft Entra ID
* User account displayed

### Evidence

![windows connection](assets/lab07-windows-connection.png)

---

## Conclusion

The device was successfully:

* Joined to Microsoft Entra ID
* Automatically enrolled into Microsoft Intune
* Verified across local system, Entra ID, and Intune

This confirms proper identity integration and MDM enrollment.
****

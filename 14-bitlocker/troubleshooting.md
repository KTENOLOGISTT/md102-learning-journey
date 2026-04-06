# Lab 14 – BitLocker Troubleshooting

## Issue: Recovery key not visible in Microsoft Entra ID

### Cause

* Delay in policy application or device sync
* BitLocker encryption completed before key upload process finished

### Explanation

BitLocker encryption and recovery key backup are separate processes.
The device may complete encryption before the recovery key is uploaded to Microsoft Entra ID.

---

### Resolution

Wait a few minutes and refresh the Entra portal.

If the key is still not visible, manually trigger backup:

```powershell
$kp = Get-BitLockerVolume -MountPoint "C:" | Select-Object -ExpandProperty KeyProtector
$recovery = $kp | Where-Object {$_.KeyProtectorType -eq "RecoveryPassword"}
BackupToAAD-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId $recovery.KeyProtectorId
```

---

## Issue: BitLocker not starting

### Possible Causes

* TPM not available or not initialized
* Device not joined to Microsoft Entra ID
* Policy not applied or device not synced
* Device restart required

---

### Resolution

Check TPM:

```powershell
Get-Tpm
```

Verify Entra join:

```cmd
dsregcmd /status
```

Force sync:

Settings → Accounts → Access work or school → Sync

Restart device if required.

---

## Key Insight

BitLocker encryption success does not guarantee recovery key backup.

Always validate:

1. Encryption status (client-side)
2. Recovery key availability (Entra ID)

Both must be confirmed for a secure and recoverable system.

### Issue – Device join and MDM enrollment failed due to incorrect initial setup

#### Symptoms

* Device could not properly join Microsoft Entra ID
* MDM enrollment did not trigger
* Join process behaved inconsistently

#### Root Cause

Windows was initially installed using a **local account**, not a work account.

This caused:

* Inconsistent identity state
* Join conflicts during manual Entra ID join
* MDM enrollment not triggering correctly

#### Resolution

Recreated the virtual machine and performed a clean setup:

* Reinstalled Windows 11
* During initial setup (OOBE), signed in with:
  [admin@emd102labs.onmicrosoft.com](mailto:admin@emd102labs.onmicrosoft.com)

Result:

* Device automatically joined Microsoft Entra ID
* MDM enrollment triggered successfully
* No further join issues occurred

#### Alternative (without reinstall)

* Run `dsregcmd /leave`
* Remove all work/school accounts
* Restart device
* Reattempt join

Note: This approach is less reliable than a clean setup and failed in this scenario.

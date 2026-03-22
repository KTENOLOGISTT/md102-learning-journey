# Lab 01 - Windows 11 Environment Setup

## Objective
Prepare a reproducible local test environment for MD-102 labs.

## Environment
- Hypervisor: VMware Workstation
- VM Name: md102-client-01

## VM Configuration (Security)
- TPM enabled
- Partial encryption (TPM required files only)

## VM Configuration
- CPU: 2 cores
- RAM: 8 GB
- Disk: 64 GB (single disk)
- Network: NAT
- Firmware: UEFI

## OS
- Windows 11 Pro

## Account
- Username: labuser
- Type: Local account
- Role: Local Administrator

## Steps
1. Created virtual machine in VMware
2. Attached Windows 11 ISO
3. Booted VM from ISO (CDROM)
4. Installed Windows 11 using custom installation
5. Created local user (labuser)
6. Renamed device to md102-client-01
7. Installed Windows updates

## Result
Windows 11 environment successfully installed and prepared for further labs.

## Notes
VM configuration is documented for reproducibility.

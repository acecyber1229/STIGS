# 💻 STIG Remediation Script: WN10-AC-000005

## 📌 Description

This PowerShell script remediates the DISA STIG **WN10-AC-000005**, which requires **disabling Microsoft accounts for sign-in** on Windows 10 systems. This control helps ensure that users cannot connect personal Microsoft accounts to domain-joined systems, supporting compliance and reducing security risks in enterprise environments.

## 🛡️ STIG Details

| STIG ID         | WN10-AC-000005 |
|-----------------|----------------|
| Description     | Windows 10 must be configured to prevent the use of Microsoft accounts for sign-in. |
| Severity        | Medium         |
| Registry Path   | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System` |
| Registry Value  | `NoConnectedUser` = `3` |
| Reference       | [STIG Viewer](https://public.cyber.mil/stigs/downloads/) |

---

## 🧪 Tested On

| Attribute        | Value                        |
|------------------|------------------------------|
| OS               | Windows 10 Pro (x64)         |
| PowerShell Ver.  | 5.1                          |
| Date Tested      | 2024-09-10                   |
| Tested By        | Adnan Mehmood                |

---

## 🚀 Usage

```powershell
# Run in an elevated PowerShell terminal
.\remediate-WN10-AC-000005.ps1


<#
.SYNOPSIS
    This PowerShell script ensures that the use of Microsoft accounts for sign-in is disabled.

.DESCRIPTION
    Enforces DISA STIG WN10-AC-000005 by setting the 'NoConnectedUser' registry value to 3.

.NOTES
    Author          : Awais C
    Date Created    : 2024-09-09
    Last Modified   : 2024-09-09
    Version         : 1.0
    STIG-ID         : WN10-AC-000005
#>

$registryPath = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System"
$valueName = "NoConnectedUser"
$desiredValue = 3

# Ensure registry path exists
if (-not (Test-Path $registryPath)) {
    Write-Output "Creating registry path..."
    New-Item -Path $registryPath -Force | Out-Null
}

# Get current value
$currentValue = Get-ItemProperty -Path $registryPath -Name $valueName -ErrorAction SilentlyContinue | Select-Object -ExpandProperty $valueName -ErrorAction SilentlyContinue

# Set value if not compliant
if ($currentValue -ne $desiredValue) {
    Write-Output "Applying remediation..."
    Set-ItemProperty -Path $registryPath -Name $valueName -Value $desiredValue -Type DWord
    Write-Output "Microsoft account sign-in disabled successfully."
} else {
    Write-Output "System is already compliant."
}

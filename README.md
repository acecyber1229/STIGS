# STIGS

<#
.SYNOPSIS
    This PowerShell script ensures that the use of Microsoft accounts for sign-in is disabled, in compliance with DISA STIG ID WN10-AC-000005.

.DESCRIPTION
    According to the DISA STIG WN10-AC-000005, systems must be configured to prevent users from adding or logging in with Microsoft accounts. This script checks the relevant registry key and sets it to the appropriate value (3) if it is not already configured.

.NOTES
    Author          : Awais C.
    LinkedIn        : www.linkedin.com/in/awais-c
    GitHub          : https://github.com/acecyber1229
    Date Created    : 2024-09-09
    Last Modified   : 2024-09-09
    Version         : 1.0
    CVEs            : N/A
    Plugin IDs      : N/A
    STIG-ID         : WN10-AC-000005

.TESTED ON
    Date(s) Tested  : 2024-09-10
    Tested By       : Adnan Mehmood
    Systems Tested  : Windows 10 Pro (x64)
    PowerShell Ver. : 5.1

.USAGE
    Example syntax:
    PS C:\> .\remediate-WN10-AC-000005.ps1

    This script must be run with administrative privileges.
#>

# Set the registry path and value to disable Microsoft account sign-in
$registryPath = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System"
$valueName = "NoConnectedUser"
$desiredValue = 3

# Check if the registry key exists
if (-not (Test-Path $registryPath)) {
    Write-Output "Registry path does not exist. Creating it..."
    New-Item -Path $registryPath -Force | Out-Null
}

# Check if the value exists and is correct
$currentValue = Get-ItemProperty -Path $registryPath -Name $valueName -ErrorAction SilentlyContinue | Select-Object -ExpandProperty $valueName -ErrorAction SilentlyContinue

if ($currentValue -ne $desiredValue) {
    Write-Output "Setting '$valueName' to '$desiredValue' at '$registryPath'..."
    Set-ItemProperty -Path $registryPath -Name $valueName -Value $desiredValue -Type DWord
    Write-Output "Remediation applied successfully."
} else {
    Write-Output "Setting already compliant. No changes needed."
}

# Uninstalling Chocolatey

Should you decide you don't like Chocolatey, you can uninstall it simply by removing the folder (and the environment variable(s) that it creates).  Since it is not actually installed in Programs and Features, you don't have to worry that it cluttered up your registry (however that's a different story for the applications that you installed with Chocolatey or manually).

## Folder
Most of Chocolatey is contained in `C:\ProgramData\chocolatey` or whatever `$env:ChocolateyInstall` evaluates to. You can simply delete that folder.

**NOTE** You might first back up the sub-folders `lib` and `bin` just in case you find undesirable results in removing Chocolatey. Bear in mind not every Chocolatey package is an installer package, there may be some non-installed applications contained in these subfolders that could potentially go missing. Having a backup will allow you to test that aspect out.

## Environment Variables
There are some environment variables that need to be adjusted or removed.

* ChocolateyInstall
* ChocolateyToolsLocation
* ChocolateyLastPathUpdate
* PATH (will need updated to remove)

## Script
There are no warranties on this script whatsoever, but here is something you can try:

**WARNING!!** This will remove Chocolatey and all packages, software, and configurations in the Chocolatey Installation folder from your machine. Everything will be GONE. This is very destructive. DO NOT RUN this script unless you completely understand what the intention of this script is and are good with it. If you mess something up, we cannot help you fix it.

***WARNING:*** Seriously, this script may destroy your machine and require a rebuild. It may have varied results on different machines in the same environment. Think twice before running this.

<!--remove
<p class="text-danger"><strong>Click the red button below to reveal the uninstall scripts.</strong></p>
<button type="button" class="btn btn-danger btn-hide">Yes, I understand the dangers of running these scripts</button>
<div id="uninstall-scripts" class="d-none">
remove-->
If you also intend to delete the Chocolatey directory, remove the `-WhatIf`:

~~~powershell
$VerbosePreference = 'Continue'
if (-not $env:ChocolateyInstall) {
    $message = @(
        "The ChocolateyInstall environment variable was not found."
        "Chocolatey is not detected as installed. Nothing to do."
    ) -join "`n"

    Write-Warning $message
    return
}
if (-not (Test-Path "$env:ChocolateyInstall")) {
    $message = @(
        "No Chocolatey installation detected at '$env:ChocolateyInstall'."
        "Nothing to do."
    ) -join "`n"

    Write-Warning $message
    return
}

$userPath = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey(
    'Environment'
).GetValue('PATH', '', 'DoNotExpandEnvironmentNames').ToString()

$machinePath = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey(
    'SYSTEM\CurrentControlSet\Control\Session Manager\Environment\'
).GetValue('PATH', '', 'DoNotExpandEnvironmentNames').ToString()

$backupPATHs = @(
    "User PATH: $userPath"
    "Machine PATH: $machinePath"
)
$backupPATHs | Set-Content -Path "C:\PATH_backups_ChocolateyUninstall.txt" -Encoding UTF8 -Force

if ($userPath -like "*$env:ChocolateyInstall*") {
    Write-Verbose "Chocolatey Install location found in User Path. Removing..."
    # WARNING: This could cause issues after reboot where nothing is
    # found if something goes wrong. In that case, look at the backed up
    # files for PATH.

    $newUserPATH = ($userPath -split [System.IO.Path]::PathSeparator).Where{
        $_ -ne "$env:ChocolateyInstall\bin"
    } -join [System.IO.Path]::PathSeparator

    [Environment]::SetEnvironmentVariable('PATH', $newUserPATH, 'User')
}

if ($machinePath -like "*$env:ChocolateyInstall*") {
    Write-Verbose "Chocolatey Install location found in Machine Path. Removing..."
    # WARNING: This could cause issues after reboot where nothing is
    # found if something goes wrong. In that case, look at the backed up
    # files for PATH.

    $newMachinePATH = ($machinePath -split [System.IO.Path]::PathSeparator).Where{
        -not [string]::IsNullOrEmpty($_) -and
        $_ -ne "$env:ChocolateyInstall\bin"
    } -join [System.IO.Path]::PathSeparator

    [Environment]::SetEnvironmentVariable('PATH', $newMachinePATH, 'Machine')
}

# Adapt for any services running in subfolders of ChocolateyInstall
$agentService = Get-Service -Name chocolatey-agent -ErrorAction SilentlyContinue
if ($agentService -and $agentService.Status -eq 'Running') {
    $agentService.Stop()
}
# TODO: add other services here

Remove-Item -Recurse -Force "$env:ChocolateyInstall" -WhatIf

'ChocolateyInstall', 'ChocolateyLastPathUpdate', | ForEach-Object {
    foreach ($scope in 'User', 'Machine') {
        [Environment]::SetEnvironmentVariable($_, [string]::Empty, $scope)
    }
}
~~~

If you also intend to delete the tools directory that was managed by Chocolatey, remove both of the `-WhatIf` switches:

~~~powershell
if ($env:ChocolateyToolsLocation) {
    Remove-Item -Recurse -Force "$env:ChocolateyToolsLocation" -WhatIf
}

foreach ($scope in 'User', 'Machine') {
    [Environment]::SetEnvironmentVariable('ChocolateyToolsLocation', [string]::Empty, $scope)
}
~~~
<!--remove
</div>
remove-->

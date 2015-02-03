# [DEPRECATED] Write-ChocolateyFailure
**NOTE**: This has been deprecated and is no longer useful as of 0.9.9. Instead please just use `throw $_` when catching errors.

`Write-ChocolateyFailure $packageName $failureMessage`

## Description
Notes an unsuccessful Chocolatey install.

## Parameters
### $packageName
This is an arbitrary name.
Example: `'7zip'`

### $failureMessage
This is the message logged back to the main Chocolatey window..
Example: `"$($_.Exception.Message)"`

## Examples
`Write-ChocolateyFailure 'StExBar' "$($_.Exception.Message)"`

[[Function Reference|HelpersReference]]
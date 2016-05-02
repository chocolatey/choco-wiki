# Get-FileName

Gets the original file name from the url. Used by Get-WebFile to determine the original file name for a file.

## Syntax

~~~powershell
Get-FileName [-url <String>] -defaultName <String> [-userAgent <Object>] [<CommonParameters>]
~~~

## Description

Uses several techniques to determine the original file name of the file based on the url for the file.

## Notes

Available in 0.9.10+.
Falls back to DefaultName when the name cannot be determined.

## Aliases

None

## Inputs

None

## Outputs

None

## Parameters

###  -Url [\<String\>]
This is the url to a file that will be possibly downloaded.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | 1
Default Value          | 
Accept Pipeline Input? | false
 
###  -DefaultName \<String\>
The name of the file to use when not able to determine the file name from the url response.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | true
Position?              | 2
Default Value          | 
Accept Pipeline Input? | false
 
###  -UserAgent [\<Object\>]
The user agent to use as part of the request. Defaults to 'chocolatey command line'.

Property               | Value
---------------------- | -----------------------
Aliases                | 
Required?              | false
Position?              | named
Default Value          | chocolatey command line
Accept Pipeline Input? | false
 
### \<CommonParameters\>

This cmdlet supports the common parameters: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer, and -OutVariable. For more information, see `about_CommonParameters` http://go.microsoft.com/fwlink/p/?LinkID=113216 .


## Examples

 **EXAMPLE 1**

~~~powershell
Get-FileName -url $url -defaultName $originalFileName

~~~

## Links

 * [[Get-WebHeaders|HelpersGetWebHeaders]]
 * [[Get-ChocolateyWebFile|HelpersGetChocolateyWebFile]]


[[Function Reference|HelpersReference]]

***NOTE:*** This documentation has been automatically generated from `Import-Module "$env:ChocolateyInstall\helpers\chocolateyInstaller.psm1" -Force; Get-Help Get-FileName -Full`.

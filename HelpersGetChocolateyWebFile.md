# Get-ChocolateyWebFile

Downloads a file from the internets.

## Syntax

~~~powershell
Get-ChocolateyWebFile -packageName <String> -fileFullPath <String> -url <String> [-url64bit <String>] [-checksum <String>] [-checksumType <String>] [-checksum64 <String>] [-checksumType64 <String>] [-options <Hashtable>] [-getOriginalFileName] [<CommonParameters>]
~~~

## Description

This will download a file from a url, tracking with a progress bar.
It returns the filepath to the downloaded file when it is complete.

## Notes

This helper reduces the number of lines one would have to write to download a file to 1 line.
There is no error handling built into this method.

## Aliases

None

## Inputs

None

## Outputs

None

## Parameters

###  -PackageName \<String\>
The name of the package we want to download - this is arbitrary, call it whatever you want.
It's recommended you call it the same as your nuget package id.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | true
Position?              | 1
Default Value          | 
Accept Pipeline Input? | false
 
###  -FileFullPath \<String\>
This is the full path of the resulting file name.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | true
Position?              | 2
Default Value          | 
Accept Pipeline Input? | false
 
###  -Url \<String\>
This is the url to download the file from.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | true
Position?              | 3
Default Value          | 
Accept Pipeline Input? | false
 
###  -Url64bit [\<String\>]
OPTIONAL - If there is a 64 bit installer available, put the link next to the other url. Chocolatey will automatically determine if the user is running a 64bit machine or not and adjust accordingly. Please note that the 32 bit url will be used in the absence of this. This link should only be used for 64 bit native software. If the original Url contains both (which is quite rare), set this to '$url' Otherwise remove this parameter.

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | 4
Default Value          | 
Accept Pipeline Input? | false
 
###  -Checksum [\<String\>]
OPTIONAL (Right now) - This allows a checksum to be validated for files that are not local

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | named
Default Value          | 
Accept Pipeline Input? | false
 
###  -ChecksumType [\<String\>]
OPTIONAL (Right now) - 'md5', 'sha1', 'sha256' or 'sha512' - defaults to 'md5'

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | named
Default Value          | 
Accept Pipeline Input? | false
 
###  -Checksum64 [\<String\>]
OPTIONAL (Right now) - This allows a checksum to be validated for files that are not local

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | named
Default Value          | 
Accept Pipeline Input? | false
 
###  -ChecksumType64 [\<String\>]
OPTIONAL (Right now) - 'md5', 'sha1', 'sha256' or 'sha512' - defaults to ChecksumType

Property               | Value
---------------------- | -------------
Aliases                | 
Required?              | false
Position?              | named
Default Value          | $checksumType
Accept Pipeline Input? | false
 
###  -Options [\<Hashtable\>]
OPTIONAL - Specify custom headers. Available in 0.9.10+.

Property               | Value
---------------------- | --------------
Aliases                | 
Required?              | false
Position?              | named
Default Value          | @{Headers=@{}}
Accept Pipeline Input? | false
 
###  -GetOriginalFileName
OPTIONAL switch to allow Chocolatey to determine the original file name from the url

Property               | Value
---------------------- | -----
Aliases                | 
Required?              | false
Position?              | named
Default Value          | False
Accept Pipeline Input? | false
 
### \<CommonParameters\>

This cmdlet supports the common parameters: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer, and -OutVariable. For more information, see `about_CommonParameters` http://go.microsoft.com/fwlink/p/?LinkID=113216 .


## Examples

 **EXAMPLE 1**

~~~powershell
Get-ChocolateyWebFile '__NAME__' 'C:\somepath\somename.exe' 'URL' '64BIT_URL_DELETE_IF_NO_64BIT'

~~~

**EXAMPLE 2**

~~~powershell

$toolsDir = "$(Split-Path -parent $MyInvocation.MyCommand.Definition)"
Get-ChocolateyWebFile -PackageName 'bob' -FileFullPath "$toolsDir\bob.exe" -Url 'https://somewhere/bob.exe'
~~~

**EXAMPLE 3**

~~~powershell

$options =
@{
  Headers = @{
    Accept = 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8';
    'Accept-Charset' = 'ISO-8859-1,utf-8;q=0.7,*;q=0.3';
    'Accept-Language' = 'en-GB,en-US;q=0.8,en;q=0.6';
    Cookie = 'requiredinfo=info';
    Referer = 'https://somelocation.com/';
  }
}

Get-ChocolateyWebFile 'package' "$(Split-Path -parent $MyInvocation.MyCommand.Definition)\thefile.exe" 'https://somelocation.com/thefile.exe' -options $options
~~~

## Links

 * [[Install-ChocolateyPackage|HelpersInstallChocolateyPackage]]
 * [[Get-WebFile|HelpersGetWebFile]]
 * [[Get-FtpFile|HelpersGetFtpFile]]


[[Function Reference|HelpersReference]]

***NOTE:*** This documentation has been automatically generated from `Import-Module "$env:ChocolateyInstall\helpers\chocolateyInstaller.psm1" -Force; Get-Help Get-ChocolateyWebFile -Full`.

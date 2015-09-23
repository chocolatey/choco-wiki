# Proxy Support For Chocolatey

## System Proxy Settings
Chocolatey by default already supports system set proxy servers 

![image](https://cloud.githubusercontent.com/assets/63502/10038284/454be026-6189-11e5-8f83-e29d1705995c.png)

## Explicit Proxy Settings
Chocolatey has proxy support starting with 0.9.9.9.

You can simply configure 1 or 3 settings and Chocolatey will use a proxy server.

# What to do if My proxy is socks?

One way to go around this is to wrap the socks proxy with an http proxy. `polipo` is one such software.

## Installing Chocolatey behind a proxy server

When trying to install Chocolatey behind a proxy server, you may be faced with errors like

> Boo to the Chocolatey gods! Get Response returned: (407) Proxy Authentication Required

Steps to help address this.

1. Copy the [install.ps1](https://chocolatey.org/install.ps1) file locally.
2. Edit the file and add proxy details (see code change). Replace the following entries with your specific proxy servers details: **PROXYSERVER:PROXYPORT; USERPASSWORD; USERNAME**
3. Ensure you have the approapriate domain prefix for the user name. Try: 'AD\UserName' or just 'UserName'.
4. Run the script to install Chocolatey.

```PowerShell
function Download-File {
param (
  [string]$url,
  [string]$file
 )
  Write-Host "Downloading $url to $file"
    #$proxy = [System.Net.WebRequest]::GetSystemWebProxy()
    $proxy = New-Object System.Net.WebProxy("PROXYSERVER:PROXYPORT", $true)
    $passwd = ConvertTo-SecureString "USERPASSWORD" -AsPlainText -Force; ## Website credentials
    $proxy.Credentials = New-Object System.Management.Automation.PSCredential ("USERNAME", $passwd);
    $downloader = New-Object System.Net.WebCLient
    $downloader.UseDefaultCredentials = $true
    $downloader.Proxy = $proxy
    $downloader.DownloadFile($url, $file)
}
```

## Windows - setting an environment variable set-up

![Open system properties](https://f.cloud.github.com/assets/396205/243504/d73aa158-8a49-11e2-96b8-6c04c1a5f06c.png)

![Set environment variable](https://f.cloud.github.com/assets/396205/243529/b9636100-8a4a-11e2-9ec6-1ea6f99c9e35.png)

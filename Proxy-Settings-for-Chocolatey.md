# Proxy Support For Chocolatey

## System Proxy Settings
Chocolatey by default already supports system set proxy servers 

![image](https://cloud.githubusercontent.com/assets/63502/10038284/454be026-6189-11e5-8f83-e29d1705995c.png)

## Explicit Proxy Settings
Chocolatey has explicit proxy support starting with 0.9.9.9.

You can simply configure 1 or 3 settings and Chocolatey will use a proxy server. `proxy` is required and is the location and port of the proxy server. `proxyUser` and `proxyPassword` are optional. The values for user/password are only used for credentials when both are present.

~~~sh
choco config set proxy <locationandport>
choco config set proxyUser <username>
choco config set proxyPassword <passwordThatGetsEncryptedInFile>
choco config set proxyBypassList "'<bypasslist, comma separated>'" #0.10.4 required
choco config set proxyBypassOnLocal true #0.10.4 required
~~~~

### Example

Running the following commands in 0.9.9.9:

~~~sh
choco config set proxy http://localhost:8888
choco config set proxyUser bob
choco config set proxyPassword 123Sup#rSecur3
choco config set proxyBypassList "'http://localhost,http://this.location/'" #0.10.4 required
choco config set proxyBypassOnLocal true #0.10.4 required
~~~

Results in the following items being added to the config file:
![image](https://cloud.githubusercontent.com/assets/63502/10038348/4205507c-618a-11e5-911b-122eb960bc53.png)

#### Explicit from CLI

Starting in 0.10.4, you can pass proxy information at runtime with each command. See 

### What to do if My proxy is socks?

It may just work. It hasn't been validated yet.

## Installing Chocolatey behind a proxy server

When trying to install Chocolatey behind a proxy server, you may be faced with errors like

> Get Response returned: (407) Proxy Authentication Required

or 

> The underlying connection was closed: Could not establish trust relationship for the SSL / TLS secure channel.

Steps to help address this.

1. Copy the [install.ps1](https://chocolatey.org/install.ps1) file locally.
2. Open a PowerShell command line.
3. Set the following environment variables - `$env:chocolateyProxyLocation` (with proxyserver:proxyport), `$env:chocolateyProxyUser` (if it is a domain account, ensure you have the appropriate domain prefix for the account, e.g. `AD\UserName` or `UserName`), and `$env:chocolateyProxyPassword` with your password.
4. With that same shell open where the environment variables are set, run the downloaded script to install Chocolatey.

### Still having trust issues? 

You've tried everything and Chocolatey still won't install - 

> The underlying connection was closed: Could not establish trust relationship for the SSL / TLS secure channel.

It could also be that your trusted root certificates are missing or not up to date (Windows 2012 does not install root certificates by default). You will need to download and install both these certificates into your LocalMachine's *Trusted Root Certification Authorities* certificate store.

To access [chocolatey.org](https://chocolatey.org):
* [Go Daddy Secure Certificate Authority - G2](https://certs.godaddy.com/repository) (File: `gdroot-g2.crt`; Thumbprint: `47BEABC922EAE80E78783462A79F45C254FDE68B`). (On the download page, this certificate is named *GoDaddy Class 2 Certification Authority Root Certificate - G2* and its listed thumbprint (`45140B3247EB9CC8C5B4F0D7B53091F73292089E6E5A63E2749DD3ACA9198EDA`) is wrong.

To access [packages.chocolatey.org](https://packages.chocolatey.org):
* [AddTrust External CA Root](https://support.comodo.com/index.php?/Default/Knowledgebase/Article/View/917/91/) (File: `addtrustexternalcaroot.crt`; Thumbprint: `02FAF3E291435468607857694DF5E45B68851868`)

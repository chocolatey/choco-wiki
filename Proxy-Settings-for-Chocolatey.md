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
~~~~

### Example

Running the following commands in 0.9.9.9:
~~~sh
choco config set proxy http://localhost:8888
choco config set proxyUser bob
choco config set proxyPassword 123Sup#rSecur3
~~~

Results in the following items being added to the config file:
![image](https://cloud.githubusercontent.com/assets/63502/10038348/4205507c-618a-11e5-911b-122eb960bc53.png)

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

It could also be that your root trusted certificates are not up to date. You will need the following certificates in your chain:

* Go Daddy Secure Certificate Authority - G2

![Go Daddy Secure Certificate Authority - G2](https://cloud.githubusercontent.com/assets/63502/13706493/ea4d0aac-e76a-11e5-913e-590dfe775fc1.png)

* COMODO ECC Domain Validation Secure Server CA 2

![image](https://cloud.githubusercontent.com/assets/63502/13706546/1a1d237a-e76b-11e5-9263-e586cef5f744.png)

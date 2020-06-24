# Chocolatey Central Mangement Upgrade
This will guide us through upgrading an existinc Chocolatey Central Management installation to the newest version (including Deployments).

___
<!-- TOC depthFrom:2 -->

- [Setup](#setup)
    - [On Chocolatey Central Management Server](#on-chocolatey-central-management-server)
        - [Step 1: Copy Packages](#step-1-copy-packages)
        - [Step 2: Upgrade Chocolatey Licensed Extension](#step-2-upgrade-chocolatey-licensed-extension)
        - [Step 3: Upgrade CCM Packages](#step-3-upgrade-ccm-packages)
        - [Step 4: Upgrade Chocolatey Agent (optional)](#step-4-upgrade-chocolatey-agent-optional)
        - [Step 5: Configure Additional Settings](#step-5-configure-additional-settings)
    - [On Endpoints](#on-endpoints)
        - [Step 1: Upgrade Chocolatey Agent](#step-1-upgrade-chocolatey-agent)
        - [Step 2: Configure Additional Settings](#step-2-configure-additional-settings)

<!-- /TOC -->

___
## Setup

### On Chocolatey Central Management Server

#### Step 1: Copy Packages
Download all the `.nupkg` files from the trial/license emails. Place these packages in the `C:\choco-setup\packages` folder. Be sure that the versions of packages you have match up with the [[Complatibility Matrix|CentralManagement#ccm-component-compatibility-matrix]].

___
#### Step 2: Upgrade Chocolatey Licensed Extension
Install or upgrade Chocolatey Licensed Extension. The order is not enforced in this release, so make sure you start with this.
Assuming you’ve place the packages in the `C:\choco-setup\packages` folder, enter the following command in an administrative PowerShell window:

    ```
    choco upgrade chocolatey.extension -y  --source "'C:\choco-setup\packages\'"
    ```

___
#### Step 3: Upgrade CCM Packages
Install or upgrade `Chocolatey-Management-Database`; then `Chocolatey-Management-Service`; then `Chocolatey-Management-Web`. **IMPORTANT:** IN. THAT. ORDER. Assuming you’ve place the packages in the `C:\choco-setup\packages` folder, enter the following commands in an administrative PowerShell window:

    First (this command is longer, as we need to cleanup and explicitly pass package parameters):
    ```
    Remove-Item -Force -Path "C:\ProgramData\chocolatey\lib\chocolatey-management-database\tools\app\appsettings.json" -ErrorAction SilentlyContinue
    choco upgrade chocolatey-management-database -y --package-parameters="'/SqlServerInstance:localhost\SQLEXPRESS'" --source="'c:\choco-setup\packages'"
    ```

    Second:
    ```
    choco upgrade chocolatey-management-service -y --source "'C:\choco-setup\packages\'"
    ```

    Third:
    ```
    choco upgrade chocolatey-management-web -y --source "'C:\choco-setup\packages\'"
    ```

___
#### Step 4: Upgrade Chocolatey Agent (optional)

If you plan on having the CCM server check into CCM as well, upgrade the `Chocolatey-Agent` package here. Assuming you’ve place the packages in the `C:\choco-setup\packages` folder, enter the following command in an administrative PowerShell window:

    ```
    choco upgrade chocolatey-agent -y --source "'C:\choco-setup\packages\'"
    ```

> :warning: **WARNING**: The Chocolatey Agent installed on the same machine that has the CCM Service installed will share the `centralManagementServiceUrl` setting, so that agent can only report into that CCM Service.

___
#### Step 5: Configure Additional Settings

Set configuration properly on your endpoints:

    ```
    choco config set CentralManagementServiceUrl https://<FQDN_CCM_SERVICE>:24020/ChocolateyManagementService
    choco feature enable --name="'useChocolateyCentralManagement'"

    # Requires Chocolatey Licensed Extension v2.1.0+, Chocolatey-Agent v0.10.0+, and Chocolatey Central Management v0.2.0+:
    choco feature enable --name="'useChocolateyCentralManagementDeployments'"
    ```

> :memo: **NOTE:** If you are upgrading, and you had the endpoint checking into CCM already, you will likely only need the last item above (i.e. you can ignore the commands to set CentralManagementServiceUrl and enable usage of Central Management)

That should get you setup and running. Just a note that you may need to adjust permissions/roles for your user if not using the default `ccmadmin` account.

___
### On Endpoints

___
#### Step 1: Upgrade Chocolatey Agent
If you plan on having the CCM/QDE server check into CCM as well, upgrade the `Chocolatey-Agent` package here. Assuming you’ve place the packages in the `C:\choco-setup\packages` folder, enter the following command in an administrative PowerShell window:

    ```
    choco upgrade chocolatey-agent -y --source "'C:\choco-setup\packages\'"
    ```

___
#### Step 2: Configure Additional Settings

Set configuration properly on your endpoints:

    ```
    choco config set CentralManagementServiceUrl https://<FQDN_CCM_SERVICE>:24020/ChocolateyManagementService
    choco feature enable --name="'useChocolateyCentralManagement'"

    # Requires Chocolatey Licensed Extension v2.1.0+, Chocolatey-Agent v0.10.0+, and Chocolatey Central Management v0.2.0+:
    choco feature enable --name="'useChocolateyCentralManagementDeployments'"
    ```

> :memo: **NOTE:** If you are upgrading, and you had the endpoint checking into CCM already, you will likely only need the last item above (i.e. you can ignore the commands to set CentralManagementServiceUrl and enable usage of Central Management)

> :warning: **WARNING**
>
> As these features have security considerations (it is enabling cross-machine communication), they must be turned on explicitly.
> If you decide you want to open this up for over the internet communication, you should also set `centralManagementClientCommunicationSaltAdditivePassword` and `centralManagementServiceCommunicationSaltAdditivePassword`.
> For more in-depth configuration options and settings for your endpoints, you can view the [[CCM Client Setup page|CentralManagemenSetupClient]]

___
[[Central Management Setup|CentralManagementSetup]] | [[Chocolatey Central Management|CentralManagement]]

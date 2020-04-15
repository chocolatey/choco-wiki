
![Image of CCM Architecture](IMAGE_URL)

## External Ports

These are ports that are opened through the corporate firewall, in case users are **not** on VPN and need to install packages from anywhere.

* Nexus Web / API - Port 8443

## Internal Ports

These are the ports that are already opened on Windows Firewall on the QDE.

* Nexus Web / API - Port 8443
* CCM Web - Port 443 
* Jenkins - 8080
* CCM Service - 24020 

### FAQs

#### Can I open up the CCM Service's port to allow machines to report in from anywhere?

While it is technically possible, we recommend using a VPN connection for client check-ins. The CCM service connection is authenticated over SSL, but our best practice recommendation is to secure the connection over a VPN as well.

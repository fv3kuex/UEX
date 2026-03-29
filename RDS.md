# User Profile Disk

### User Profile Disk

The User Profile Disk is an alternative for Roaming profiles and folder redirection in Remote Desktop services scenarios. It uses VHDX or VHD file stored on a central network share that is dedicated to an individual user account within a specific RDS collection. This virtual disk contains the entire contents of the user profile directory, which allows for high-speed mounting and immediate availability of data upon session initiation.

When an UPD is enabled a template called UVH.template.vhdx file is created in the file share.

User Profile Disks function at the collection level, meaning that a disk created for a user in one collection is unique to that collection and will not be utilized if the user connects to a different collection within the same RDS deployment. The collection settings define the UNC path for storage, the maximum size of the disk, and specific folders to be included or excluded from the virtualization process.

### Step by Step Operational Process

1. The user initiates a connection to the RDS deployment, and the Remote Desktop Connection Broker directs the request to an available Session Host within the specified collection.  
2. The Session Host identifies the user's Security Identifier and locates the corresponding VHDX file on the designated network share.  
3. The Session Host establishes an exclusive SMB connection to the file server and mounts the virtual disk as a local volume.  
4. Throughout the session, all changes to the desktop, documents, and application settings are written directly to the VHDX file in real-time.  
5. Upon a successful sign-out, the Session Host closes all open file handles, unmounts the virtual disk, and releases the file lock on the network share.

### Advantages of User Profile Disks

- It is primarily used to avoid profile corruption.  
- Upon sign-out, the Session Host unmounts the virtual disk and releases the exclusive file lock on the network share.  
- Profile corruption is avoided because the virtual hard drive is exclusively mounted and cannot be simultaneously accessed from multiple session hosts.  
- Configuration and deployment are simple, resulting in reduced logon and log-off times.  
- User Profile Disk storage flexibility allows VHDX files to be hosted on high-availability cluster volumes, high-performance storage area networks, or standalone drives.  
- UPD works with a single collection only, a user connecting to two different collections will have two different profiles.

# CredSSP

Credential Security Support Provider is a security protocol that facilitates the secure transfer of user credentials from a client machine to a remote server during the initial phase of a Remote Desktop Protocol connection. It is the underlying technology that enables Network Level Authentication, ensuring that the authentication process is completed before the server allocates significant resources or presents the full Windows login screen to the user.

### **Credential Security Support Provider**

The Credential Security Support Provider is a specialized authentication provider that creates a secure, encrypted tunnel between the local client and the remote host using Transport Layer Security. This mechanism allows the client to "delegate" or pass the user's login information safely to the remote server, which then processes the logon request on the user's behalf without exposing the credentials to potential eavesdropping on the network.

### **Authentication Integration**

CredSSP functions by wrapping other authentication protocols, such as Kerberos or NTLM, within its secure encrypted channel. When a user attempts to connect to an RDS environment, CredSSP acts as the transport envelope that carries the authentication tokens to the RD Session Host or RD Gateway, verifying that the user has the appropriate permissions to access the remote system before the graphical user interface is ever initialized.

### **Step by Step Operational Process**

1. The client initiates a connection request to the Remote Desktop Session Host.  
2. The client and server perform a TLS handshake to establish an encrypted communication channel.  
3. The user enters their credentials into a local dialogue box on the client machine rather than on the remote desktop.  
4. CredSSP encrypts these credentials and transmits them through the established TLS tunnel to the remote server.  
5. The remote server receives the encrypted package and uses its Security Support Provider to validate the credentials against the Active Directory Domain Controller.  
6. Once the identity is confirmed, the server grants the connection and proceeds to build the user's remote session environment.

### **Components of CredSSP**

1. Transport Layer Security: The cryptographic protocol used to encrypt the communication pipe through which credentials travel.  
2. Network Level Authentication: The RDS feature that utilizes CredSSP to force authentication at the start of the connection.  
3. TSPKG: The specific Security Support Provider DLL within the Windows operating system that handles the CredSSP logic.  
4. Simple and Protected GSS-API Negotiation Mechanism: The protocol used to negotiate which underlying authentication method will be used within the CredSSP tunnel.

### **Advantages of CredSSP**

1. Mitigation of Denial of Service: Prevents unauthorized users from consuming server RAM and CPU by blocking session creation until valid credentials are provided.  
2. Credential Protection: Ensures that sensitive password hashes or Kerberos tickets are never sent across the network in an unencrypted state.  
3. Single Sign-On Experience: Facilitates the automatic passing of current desktop credentials to the remote host, reducing the need for multiple login prompts.  
4. Improved Server Security: Shields the remote server's login process from being directly exposed to the internet or untrusted internal networks.

# Connection Flow

The Connection Flow in a Remote Desktop Services environment without a Gateway defines the sequence of network communications and component interactions required to establish a remote session. This process relies on specific port configurations and the coordination between Web Access, Connection Broker, and Session Host roles to deliver applications and desktops to the end user.

### **Connection Flow RDS (Without Gateway)**

The Connection Flow is the architectural path an authentication and resource request follows from the client device to the final session host. In a deployment lacking an RD Gateway, the client communicates directly with internal roles, necessitating that the appropriate ports are reachable within the network premises to facilitate the handshake between the user and the RDS infrastructure.

### **RD Connection Broker Services**

The RD Connection Broker utilizes two primary sub-services to manage the environment: the Remote Desktop Connection Broker Service (TSSDIS), which handles terminal services session tracking and load balancing, and the Remote Desktop Management Service (RDMS), which is responsible for displaying and managing the overall RDS deployment overview.

### **Step by Step Connection Process**

1. The user initiates a connection by entering a specific URL, such as `https://rdwa.domain.com/rdweb`, which utilizes port 443 for secure HTTPS communication.  
2. A firewall check occurs to determine if the user is permitted to enter the premises; if allowed, the request reaches the RD Web Access role.  
3. RD Web Access utilizes Internet Information Services (IIS) to perform form-based authentication, reaching out to Active Directory to verify if the user should be granted access.  
4. Communication over port 5504 (RADCM Services) creates a tunnel that allows RD Web Access to retrieve information from the Connection Broker regarding which specific app resources are assigned to the user.  
5. The user views the available resources on the RDWA portal; clicking an application triggers the download of an `.rdp` file containing authentication settings, gateway details, and the FQDN of the Connection Broker.  
6. The user launches the `.rdp` file, which reaches out to the RD Connection Broker on port 3389\.  
7. The Connection Broker receives the request, utilizes Windows Internal Database (WID) information to perform load balancing, and points the connection to the most suitable available RD Session Host.

### **Components of the Connection Flow**

1. Internet Information Services: The web server engine that hosts the RD Web Access portal and manages the initial user authentication handshake.  
2. Port 443: The standard network port for encrypted web traffic used by clients to access the initial RD Web interface.  
3. Port 5504: The specific port used for Remote Desktop Connection Broker Management services to communicate with Web Access.  
4. Port 3389: The default port for the Remote Desktop Protocol used when the client attempts to establish the final session with the broker or host.  
5. Port 5985: The port used for Windows Remote Management (WinRM) communications, which is utilized for deployment management rather than the actual connection establishment.

RDCB Service TSSDIS : It is responsible for Connection brokering and Session routing. i) Disconnected session reconnection. ii) Load balancing \- Helps distribute incoming connections across RD session Host in a collection.

RDS Connection flow with Gateway.

4). In the downloaded RDP file the gateway server is specified. policies / restrictions can be applied. Such as connection authorisation policy (CAP) Resource authorisation Policy (RAP). CAP: Connection Authorization Policies define weather the external user gets the connection to the session host RAP: Resource Authorization Policies- defines weather the external user is allowed to the physical Resources.

RDGW \-\> Remote desktop Gateway is friendly name for TS Gateway. both are service for Gateway.

RDS Connection flow with Licensing.

6. RDLS and RDSH communicates on port TCP-135 Communication between the license server and AD happens on port TCP-389

RDLS provides licenses (CAL) \-\> Needs to be purchased from microsoft after the grace period is over.

Grace period is of maximum 120 days Client Access licenses \-\> Per user \-\> Per device.

# Remote Desktop Licensing

Remote Desktop Licensing is a specialized server role within the Remote Desktop Services architecture that manages the Client Access Licenses required for users or devices to connect to an RD Session Host. This system ensures legal compliance with Microsoft software terms by tracking the issuance, expiration, and renewal of licenses within a deployment.

### Per Device RDS CAL

The Per Device RDS CAL is a licensing model where a license is physically assigned to each individual hardware device that connects to the Remote Desktop Session Host. This model is typically utilized in environments where multiple employees share the same workstations, such as in shift-based call centers or manufacturing floors.

* Physically assigned to each device  
* Tracked by License Server  
* Can be tracked regardless of AD memberships  
* You can revoke up to 20% of RDS CALs  
* Temporary RDS CALs are assigned on first sign-in per device and are valid for 90 days  
* Permanent RDS CALs are valid for a random period of 52-89 days before renewal  
* Can't be over-allocated

### Per User RDS CAL

The Per User RDS CAL is a licensing model where a license is assigned to a specific user account in Active Directory, allowing that individual to access RDS resources from any number of different devices. This model is most effective for mobile workforces or office environments where users switch between laptops, desktops, and home computers.

* Assigned to a user in Active directory  
* Tracked by License Server  
* Can't be tracked within a workgroup  
* You can't revoke any RDS CALs  
* Temporary RDS CALs aren't available  
* Permanent RDS CALs are valid for 60 days before renewal and 90 days before reassignment  
* Can be over-allocated in breach of the Remote Desktop License agreement

### Components of RDS Licensing

1. Remote Desktop Licensing Service: The core service (TermServLicensing) that manages the license database and responds to license requests from RD Session Hosts.  
2. Grace Period: A 120-day window provided to a newly installed RD Session Host during which connections are permitted without an active license server.  
3. RD Licensing Manager: The administrative tool used to activate the license server, install RDS CAL packs, and monitor license usage.  
4. TSCPUBRPC: The Remote App and Desktop Connection Management service that pulls information of the published resources from the Connection Broker on port 5504\.

### Advantages of RDS Licensing Models

* Compliance Management: The license server provides a centralized point to ensure the organization remains within the legal boundaries of its Microsoft agreement.  
* Cost Optimization: The availability of both Per User and Per Device models allows IT departments to choose the most economical path based on their specific hardware-to-user ratio.  
* Administrative Flexibility: The 2-session administrative limit allows for server maintenance via mstsc /admin without necessitating a commercial CAL.  
* Automated Tracking: The license server handles the complex task of tracking expiration dates and renewal cycles (such as the 52-89 day random period for devices).  
* Scalability: License servers can be authorized at the domain level to provide CALs to multiple RD Session Host farms across the entire enterprise.

mstsc /admin: This can be used by admins to perform administrative. Connect to a Session Host that is a part of a RDS deployment to perform administrative tasks as mstsc in such scenarios is not supported.

* Licensing can be configured through RDS deployment overview or can be configured on individual Session host through group policy.

Com. Management / Administrative template / Windows component / Remote desktop services / RDSH / Licensing.

• Use the specified Remote desktop.

• Type of Licenses.

RD CALs can be of two types:

* Per User \-\> When users are less and devices are more.  
* Per device \-\> When devices are less and users are more.  
* The Session host sends request to licensing server over port TCP-135 to check whether the client has valid CAL or not.  
* License Server checks with AD whether the client is authentic or not. This communication happens over port-389 (LDAP).  
* Licensing server database contains user details, issuer data, expiry date of license, CAL type etc.  
* If a Session is disconnected and not logged off, when connection broken connects the user back to the Session host using the database it has it does not contact license server to validate CALs.  
* If user A takes session and temp CAL is assigned to the device. after 80 days if the same user connects to the Session host using the same device CAL is assigned if the user connects after 90 days.  
* If after 52 days of permanent of CAL i.e. expiry of a user logs in through same device. A new CAL is assigned if it is expired.  
* 7 days before the expiry eg: valid for 52 days i.e. 45 days. A user comes in session, Session host contacts license server to check if CAL is available it will ask to renew the cal.  
* In case of permanent device Cals with more than 7 days remaining in the expiry license server is not contacted. 

# CAL and TSLG

\-\> Hardware ID Store:

\-\> Sub-Keys which store the information about the license.

\-\> Hardware ID stores a random 20-byte identifier specific to that client machine and is generated automatically by windows.

CAL Version of CAL

16 2016

19 2022

22 2019

25 2025

CAL should be compatible with Session host and the licensing server.

Backwards Compatibility

CALs are compatible with older or same version of Session Host

eg.

CAL-2022

SH2016 (Checkmark)

SH2022 (Checkmark)

SH-2025 (X)

Forward Compatibility

Same or older version of CALs are compatible with same or newer version of License Server.

CAL-2019

LS-2016 (X)

LS-2019 (Checkmark)

LS-2022 (Checkmark)

LS-2025 (Checkmark)

* AD is must for per user licensing.

LKPID

License Key Pack ID, it is needed for License server installation and activation and for CAL installation.

LSID \-\> License Server ID \-\> It is provided when licensing server is activated.

Built-in over user is a reporting mechanism to tell that these number of users have logged in without an appropriate CAL.

By default a license server attempts to provide the most appropriate RDS CAL for a connection.

Built-in overused group comes into picture only if the licensing mode is set to per user.

* When a device is used to login for the first time license server issues a temp CAL. This temp CAL has a hardware ID associated with it. Next time when the user requests for sessions through same device, hardware ID is also sent along. The license server checks hardware IDs and issues a permanent CAL to device. Now stores the permanent CAL with hardware ID.  
* Terminal Server License Server Group.  
  This is a group in which we have to add domain controller group so that we can install CALs in per user scenario.

Whenever we add session host role to a server, remote desktop diagnostic information can be seen where we can check number of CALS available, types of CAL, name of the license server etc. This tool is called Licensing Diagnoser.

mstsc /admin: This can be used by admins to perform administrative. Connect to a Session Host that is a part of a RDS deployment to perform administrative tasks as mstsc in such scenarios is not supported.

* Licensing can be configured through RDS deployment overview or can be configured on individual Session host through group policy.

Com. Management / Administrative template / Windows component / Remote desktop services / RDSH / Licensing.

• Use the specified Remote desktop.

• Type of Licenses.

RD CALs can be of two types:

* Per User \-\> When users are less and devices are more.  
* Per device \-\> When devices are less and users are more.  
* The Session host sends request to licensing server over port TCP-135 to check whether the client has valid CAL or not.  
* License Server checks with AD whether the client is authentic or not. This communication happens over port-389 (LDAP).  
* Licensing server database contains user details, issuer data, expiry date of license, CAL type etc.  
* If a Session is disconnected and not logged off, when connection broken connects the user back to the Session host using the database it has it does not contact license server to validate CALs.  
* If user A takes session and temp CAL is assigned to the device. after 80 days if the same user connects to the Session host using the same device CAL is assigned if the user connects after 90 days.  
* If after 52 days of permanent of CAL i.e. expiry of a user logs in through same device. A new CAL is assigned if it is expired.  
* 7 days before the expiry eg: valid for 52 days i.e. 45 days. A user comes in session, Session host contacts license server to check if CAL is available it will ask to renew the cal.  
* In case of permanent device Cals with more than 7 days remaining in the expiry license server is not contacted. 

# Services in RDS and Certificates

Services in RDS.

i) RDCB : TSSDIS : RDM

ii) RDSH : TermService (Remote Desktop Services)

iii) RDGW : TS Gateway.

iv) RDWA : TSCPubRPC

Certificates (certlm.msc)

To ensure validity of clients, certificates are required.

User needs to install certificates for authentication before connecting to service server. Certificate identifies whether client is authentic.

There are two types of certificate.

1. Self-Signed Certificate: Certificate authority creates a Certificate (Valid for 12 months)  
2. Trusted Certificates: Purchased from third party vendors, such as: GoDaddy, Hostinger or public certificate authority and validity varies as per users choice and budget.

Certificate is needed by gateway and in case Internal users, self signed certificates may be used but not necessary.

Certificate is created through Server Manager.

CN (Fqdn) (Certificate name)

SAN (Subject alternate name)

List of other Servers

It is always recommended to create a certificate using web access name to see all the—

To see all the certificates of all the roles we have option in Server manager in Server manager to create self-signed certificate.

Since it is needed by gateway the name should be of gateway for example RDWA.Contoso.com

SAN: It should include names of all server connected to each other (CB, LS, SH)

Domain name it is called wild card entry we have to name on SAN with this name in case there are 5 or more servers

A Full blown session is when the user has access to whole desktop and not limited apps on session host

* RDS requires certificates for  
  (i) Server authentication  
  (ii) Establishing secure connections  
  (iii) Single Sign On  
* .pfx is a type of certificate extension. Certificates that are required to be installed for RDS, RDS roles should be in .pfx format.  
* Certificates can also be deployed using AD certificate service ADCS for the domain.  
  A certificate from a public CA is required for external users to connect.


# Remote desktop easy print

Remote desktop easy print.

It is used for printing on remote computer with redirected printers (TS easyprint)

It restricts/prevents new connections on the session host if turned on.

Graceful sign out: User needs to sign out properly from the session host that is called.

qwinsta: This command line is used to check the active sessions on a session host. RDS deployment overview, gateway, qwinsta

Active sessions can be checked on: RDCB \-\> Collection \-\> Connection. RDGW \- Session host \-\> run cmd "qwinsta"

# RD Gateway

RD Gateway.

Users can access internal network and RDS resources from remote locations without using a VPN

\-\> It is used to configure maximum number of simultaneous connections

\-\> Disable new connections  
\-\> Can configure certificates to secure connection communications between client and RD gateway.

\-\> The RD client must trust the certificate so it is recommended to use third party public CA certificate or internal CA

\-\> Configure http port

\-\> HTTP transport uses SSL to establish secure connection between RD client and the RDP server through Gateway.

\-\> Using RD gateway we can also enable or disable UDP transport.

# Transport matrix

Transport matrix

Server

Server 2008 RD Gateway: RPC over HTTP

Server 2008 R2 RD Gateway: RPC over HTTP transport

Server 2012 / 2012 R2 RDGW: RPC over HTTP, HTTP and UDP.

Client

RDP 7.1: RPC over HTTP transport

RDP 8.1: HTTP is default, fallback to RPC over HTTP if HTTP is not available.

RDP 10.2 (WIN 10\) will use HTTPS first (to web sockets to virtual directory (RDGW) then revert to RPC over HTTP.

Local server running over NPS: Policies are stored on the RD gateway server.

Central server running NPS: Policies are stored in a central server that is running NPS.

We can add computers with RD Gateway roles installed using the server farm on the gateway server.

We can also add gateway server from the server manager from the RDS deployment overview.

The new RD gateway servers to the RD gateway and server farm must be domain members and they must each have identical RD CAPS and RD RAPS.

# SSLbridge / Messaging / UPD

SSL bridging device can enhance security by terminating SSL sessions, inspecting packets and re-establishing SSL sessions.

You can configure in two ways.

* HTTPS \- HTTPS bridging. In this configuration the RD gateway client initiates SSL (HTTPS) request to the SSL bridging device initiates a new HTTPS request to the RD gateway server for maximum security.  
* HTTPS \- HTTP bridging. In this configuration the RD gateway client initiates an SSL (HTTPS) request to the SSL bridging device. The SSL bridging device initiates a new HTTP request to the RD gateway server.

Messaging. : Configure system message. : Configure logon message.

* Only allow connections from RD clients that support RD gateway messaging. Enabling this setting will prevent the users that are using versions older than Remote Desktop Connection (RDC) 7.0 from connecting.

User profile disk (UPD) is an alternative to roaming profiles and folder redirection in RDS scenarios.

* UPD centrally stores user and application data on virtual disk (vhd, vhdx) eg. when a user logs on their profile disk is attached to their session and detaches upon logout.  
* When UPD is enabled, a template called UVHD-template.vhdx file is created in the file share.  
* For every new user that logs on a new vhdx file is created based on the template.

Advantages

1. Avoiding profile corruption.  
2. Configuration and deployment simple.  
3. Logon and logoff time are reduced.  
4. Previously profiles could be corrupted if used simultaneously on multiple computer simultaneously avoiding profile corruption.  
5. UPD can be stored on a SMB cluster shared volume, SANs or local storage.  
6. UPD are for single collection only a user connecting to two different collections will have two separate profiles.

# RDS Auth / Kerberos / NTLM

# RDS Authentication

## Kerberos

Kerberos is a network authentication protocol that uses tickets and asymmetric key cryptography to eliminate the need to transmit passwords over the network. It is built into Active Directory (AD) and is used to authenticate users to network resources. In an RDS environment, Kerberos is utilized with Network Level Authentication (NLA) when users are first authenticated to their domain and subsequently authorized to access RDS resources.

## NTLM

In the NTLM authentication protocol, the resource server contacts the domain controller for computers on the user account domain. It serves as the default authentication protocol on the RD Gateway, as Kerberos is not supported over a WAN.

# Certificates

Certificates are digital keys used to identify servers. Remote Desktop Services uses certificates to sign the communication between two computers. When a client connects to a server, the identity of the server and the information from the client are validated using these certificates.

# RDS Deployment Properties

* Gateway: Configure settings such as the gateway name.  
* Licensing: Manage license configuration and specify the license server.  
* Web Access: Manage the web address for the deployment.  
* Certificates: Create and manage digital certificates for the environment.

# Collection Properties

(i) General We can change the name of collection from collection properties.

(ii) We can control security groups, we can choose which user gets the access to the collection by adding removing user/groups.

(iii) We can manage session configurations such as disconnected session timeouts, idle session / Active session limits. etc.

(iv) We can configure the security layer and encryption level

(v) Configure load balancing.

## Security Layer

The security layer determines how the client and server negotiate authentication before establishing an RD session. By default, RDP uses an encrypted channel. The available security layers include:

1. RDP Security (Low): Basic encryption level.

## Encryption Level

Specifies how the data sent from the client to the server is protected during transmission.

# RDS Licensing and Connection Flow

Details regarding the different types of Client Access Licenses (CALs) and the overall RDS connection flow.

What service is responsible for RDS deployment overview.

# Remote Assistance

Remote Assistance is a tool that enables a trusted person to remotely and actively assist someone with a computer problem. With the user's permission, the helper can take control of the computer and perform tasks remotely. This can be launched using the command *msra.exe /offering*.

# Collections

Collections

* It is a set of (pool of) RD session hosts. A session collection consisting of one or more RD session hosts.  
* Multiple session host can be part of a collection but same session host cannot be part of multiple collection.  
* We can configure the published resources from a collection. eg: publish a remote app, unpublish.

Collection properties:

(i) General We can change the name of collection from collection properties.

(ii) We can control security groups, we can choose which user gets the access to the collection by adding removing user/groups.

(iii) We can manage session configurations such as disconnected session timeouts, idle session / Active session limits. etc.

(iv) We can configure the security layer and encryption level

(v) Configure load balancing.

(vi) We can configure basic RDP settings such as clipboards, printer redirections etc.


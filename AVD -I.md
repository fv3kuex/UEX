Q what is cloud.?

The cloud refers to a network of remote servers owned and managed by third-party providers, accessible over the internet. It provides on-demand access to computing resources and services like storage, Virtual machines, application and networking etc.

Types of cloud computing Services:

i) Infrastructure as a Service (IAAS): Users manage their own operating systems, software, data while provider manages the infrastructure.

ii) Platform as a Service (PAAS): Users manage their applications while the provider manages underlying infrastructure and platform tools.

iii) Software as a Service (SAAS): Users access software applications hosted and managed by provider.

Types of cloud:

i) Public Cloud: These are owned and operated by third party cloud service providers, which deliver computing resources like Virtual Machines, storage, networking etc over the internet. Microsoft Azure is an example of a public cloud. With a public cloud, all hardware, software and other supporting infrastructure is owned and managed by the cloud provider.

ii) Private Cloud: It refers to cloud computing resources used exclusively by a single business or organization. A private cloud can be physically located on company's on site datacenter. Some companies also pay third party service providers to host their private cloud. A private cloud is one in which services and infrastructure are maintained on a private network.

iii) Hybrid cloud: It combines public and private clouds bound together by technology that allows data and applications to be shared between them. By allowing data and applications to move between public and private clouds, a hybrid cloud gives your business greater flexibility and more deployment options.

Azure:

Azure is a cloud computing platform offered by Microsoft that provides a wide range of services for building, deployment and managing applications and infrastructure. These services include compute, storage networking analytics, AI/ML and more. Azure allows you to access and manage these resources through a web-based portal or through various tools eg: nerdio, Terraform.

Tenant ID: In Azure it is a globally unique identifier that is assigned to an organization that represents a specific organization within Microsoft Entra ID. We can find it in Azure portal under the Microsoft Entra ID section.

Resource Group: It is a container that logically groups related resources allowing for coordinated management and deployment of those resources.

Vnet: It stands for Virtual Network used to create a logically isolated section in Microsoft Azure and securely connect it outward.

Host pool: It is a collection of virtual desktop.

Azure Virtual Desktop:

Azure Virtual Desktop (AVD) is a desktop and app virtualization service that runs on Azure.

Pre-requisits for creating AVD:

* An Azure account with an active subscription.  
* A supported identity provider. (Entra and AD)  
* A supported OS for session host.  
* Network connectivity (Virtual Network)  
* Appropriate License  
* Windows App

AVD Components:

1. RD Broker: (Remote Desktop Broker): Orchestrates incoming connection.  
2. RD Diagnostics: As the name suggests it helps diagnose issues.  
3. RD Gateway: Web socket service for RDP connectivity.  
4. RD Web: The user-facing website and endpoint. It returns the connection information to the user's device.  
5. Geographical Database: Contains the connection files (.rdp) and icons for every resource that a user has been provisioned.  
6. Diagram Description: The diagram shows the workflow for Feed Discovery and Connection.  
* A client (Windows/MAC/Android) connects to AAD to get an AAD Token.  
* The client communicates with RD Web (Feed discovery) over port 443 using the token.  
* RD Web communicates with RD Broker.  
* RD Broker interacts with the Resource Directory and Geographical Database.  
* For session connectivity, the client connects to RD Gateway (Web Socket).  
* The RD Gateway connects to the RDSH (Remote Desktop Session Host) via a reverse connection.  
* RDP Shortpath (UDP) via port 3478 is shown as an alternative path involving a UDP Relay and AFD.

AVD \+ AAD

1. Session host maintains permanent outbound connections to RD broker.  
2. User requests feed using Windows App / Web browser.   
   a) User authenticates with azure active directory and gets AAD Token.   
   b) User presents token to RD Web.   
   c) RD web queries RD broker (with token) for resources.  
    i) RD broker checks with the resource directory for geographical databases.  
   ii) The Geographical database contains the connection files (.rdp) and icons for every resource that the user has been provisioned.  
   iii) The RD broker service returns the .rdp files and application icons to the web service.  
   iv) The RD web service returns this information to the users device.  
3. User initiates connection using the .rdp file:  
   a) The remote session begins with a connection to Azure Front Door, which provides the global entry point to AVD.  
   b) Azure Front Door directs the connection to the gateway service with the lowest latency.  
   c) Gateway service queries the broker service for session hosts (with AAD token)  
4. The broker orchestrates the connection between users device and the session host.  
   a) The broker service returns the session host to the same gateway service.  
5. Depending on the configuration and available network protocols, connection is made using below.  
   a) Reverse Connect transport: after both client and session host connected to the gateway service, it starts relaying the RDP traffic using TCP between the client and session host.  
   b) RDP shortpath: A direct User Datagram Protocol (UDP)-based transport is created between the users device and the session host, bypassing the gateway service.

Note: Reverse connect transport is the default connection type.

AVD Components:

* Managed by Microsoft  
1. RD Web Service  
2. RD Broker  
3. RD Gateway  
4. RD Resource directory  
5. RD Geographical Database  
6. Azure Diagnostics  
* Customer-managed Components:  
1. Session host: The actual VM where users connect  
2. User identities (AD):  
3. Tenant:  
4. File share/File Server:  
   

Deployment Steps (Microsoft \- Entra Joined)

* Create a resource group.  
* Under the resource group create a Virtual-network (V-net).  
* Create a host pool, Before that we have to create NAT gateway and associate with the subnet.  
* Create a workspace.  
* Create a Virtual Machine.  
* Create users.  
* Assign users to the application assignment.  
* Assign roles to users. (Desktop Virtualization User, Virtual Machine User Login) These are RBAC roles.

\[Imp\]: In Entra joined Scenario, below RDP property needs to be added to the Host pool RDP properties target is aadjoined:i:1 0 \= False enablerds aadauth:i:1 1 \= True

\> AVD Listner: SXS stack listner must be up and running all the time to establish connection. (check from qwinsta)

\> Two types of deployment (domain joined) i) Microsoft Entra ii) Hybrid Active Directory

\> Each host pool consists of one or more session hosts.

\> There are two types of host pools

* Personal. In personal each session host is assigned to an individual user. Personal host pools provide dedicated desktops to end-users that optimize environments for performance and data seperation.  
    
* Pooled: User sessions can be load balanced to any session host in the host pool. There can be multiple different users on a single session host at the same time. Pooled host pools provide a shared remote experience to end-users, which ensures lower costs and greater efficiency.

Note\*: A change was introduced on March 31st 26 which by default sets the VNET to private network. To allow public access we need to create the NAT gateway.

One Nat gateway can be associated with multiple subnets but one subnet cannot be associated with multi NAT gateways.

Application group: An application group is a logical grouping of applications that are available on session hosts in a host pool.

Types of Application group:

(DAG) Desktop: Users access the full Windows desktop from a Session host, available with pooled or personal host pools.

RemoteApp Application Group (RAG): Users access individual applications you select and publish to the application group. Available with pooled host pools only.

Workspaces: A workspace is a logical grouping of application groups. Each application group must be associated with a workspace for users to see the desktops and applications published to them.

An application group can only be assigned to a single workspace.

AVD Agents: AVD Agents acts as the intermediate communicator between the AVD service and the virtual machines, enabling connectivity.

* Remote Desktop Agent boot loader: Executable that loads the agent.  
* RD Services SxS Network Stack: responsible for maintaining connection with RD Broker service and gateways. Also listens for incoming connection requests.  
* RD Services Infrastructure Agent  
* RDS Infrastructure Geneva Agent: It is a monitoring agent used for monitoring agents.

Note:

* AVD uses TLS 1.2 for connection.  
* AVD Agents could be downloaded from microsoft public article.  
* Stack heartbeats: sends heartbeats every second.  
* Agent heartbeat: sends heartbeat every 30 seconds.  
* If the RD broker misses the heartbeat it recognises that the session is unhealthy and stops sending data to the client.  
* Missing eight heartbeats will trigger a warning state. (Session is unhealthy)  
* RDS CALs are required for RDS licensing on AVD with Server OS.  
* Microsoft licence (E3, E5) is sufficient for AVD running client OS.

Start VM on Connect: It lets you reduce cost by enabling users to turn on their session host (VMs) only when they need them.

Setup "Start VM on Connect": i) Start VM on Connect can be setup for both personal and pooled sessions. ii) A role is needed for this "Desktop Virtualization Power On Contributor". This role is assigned on the subscription. This could be setup from Azure portal and Powershell/CLI.

To add an existing standalone VM to an existing host pool: 

i) Connect to the VM. 

ii) Navigate to Control panel. 

iii) Check for AVD agents and uninstall them. (listener is last) 

iv) Download the AVD agents from public article. 

v) Install the AVD agent bootloader followed by RD agent. 

vi) When installing the RD agent a registration key will be needed to register it to the host pool. vii) The registration key can be found or generated from the Host pool overview (generate key). 

After successful installation of the agents the VM should be added to the host pool.

What parameters are checked for health of VMs in AVD?

i) Domain joined check

ii) Domain trust check

iii) SxS stack listner check

iv) Monitoring agent check

v) Metadata check

vi) App attach health check

vii) URL accessible check

viii) Fslogix health check

Reverse Connect:

AVD users reverse connect transport using outbound connectivity to the AVD infrastructure (broker service, gateway) over https (443) connection.

Forward Connect:

It requires an inbound 3389 port to be opened on the RD tenant. Session host receives an incoming request over the RDP port (3389).

Difference between AVD and RDS

Object Comparison

RDS:

Deployment Type: Session-based or virtualization-based deployment

Collection/Pool: Session-Collection

Published Desktop: Published desktop

Published Apps: Published remote apps

OS Support: Windows Server OS is used with session host role, no other OS is used

AVD:

Deployment Type: Tenant

Collection/Pool: Host pool

Published Desktop: Desktop app group (DAG)

Published Apps: (RAG) Remoteapp Application Group

OS Support: Can use Client Operating System (Single session/multisession) and Server OS can also be used. Client OS \= Windows 11 Enterprise

Differentiation of AVD and RDS

1. Image Composition  
2. Semi-annual channel releases only.  
3. Windows desktop Search optimised for multi-session mode.  
4. Windows defender optimised for multi-session mode.  
5. No Server manager.  
6. No support for single box deployment.  
7. Inbound RDP ports are not required.

Load balancing types: 

i) Breadth First: Breadth first aims to evenly distribute new users' sessions on session hosts in a host pool. You don't have to specify a maximum session limit for the number of session. 

ii) Depth first: Depth first keeps starting new user sessions on one session host (with maximum session) until the maximum session limit is reached.

(Portal) Host pool \-\> Settings \-\> RDP properties \-\> Advance \[Property path\] Host pool \-\> Networking \-\> RDP shortpath \-\> \[RDP shortpath configure Path\] Host pool \-\> AutoScaling

Create a resource \-\> Search Scaling plan \-\> Configure Scaling plan.

Autoscale: Autoscale lets you scale your session host virtual machines in a host pool up or down to according to schedule to optimize deployment costs. Dynamic autoscaling is only available in Azure and isn't supported in Azure Government.

* Scaling plan can be configured for Ramp-up and Ramp-down.  
* Scaling plan can be assigned to multiple host pools in the same resource group.  
* A host pool can have only one scaling plan assigned to it.  
* Scaling plan diagnostics can be enabled for troubleshooting purposes.  
* A scaling plan can have multiple schedules.  
* To assign a scaling plan "Desktop Virtualization Power On Off contributor" role needs to be assigned to service principle (AVD) on the Subscription.

AVD Definition:

Azure Virtual Desktop is a desktop and application virtualization service which runs on Azure.

AVD Function:

* Full windows experience: AVD delivers a full windows experience with windows 11 or Windows Server.  
* Single and Multi-Session Compatibility: You can assign a machine to single user by using single session or use multi-session for scalability.  
* Remote App: Offer full-desktop or use RemoteApp to deliver individual apps.  
* Scalability: Scalable and flexible environment could be created

Q What is subscription.

A subscription is an agreement with microsoft to use one or more Microsoft cloud platforms or services, for which charges accrue based on either a per-user license fee or on cloud-based resource consumption.

* Microsoft's Software as a Service (SaaS)-based cloud offerings (Microsoft 365 and Dynamics 365\) charge per-user license fees.  
* Microsoft's Platform as a service (PaaS) and infrastructure as a service (IaaS) cloud offerings (Azure) charge based on cloud resource consumption.

Q What is a Tenant?

A tenant represents an organisation in Azure AD. It is a dedicated Azure AD service instance that an organisation receives and owns when it signs up microsoft cloud services such as Intune, Ms Azure, Ms 365 and 365 B etc.

Each AD tenant is different.

Q What are the requirements for Tenant Setup?

(i) An Azure Subscription on an Active account. (ii) Azure active directory tenant ID. (iii) Global administrator account within the Azure AD tenant.

Q What is infrastructure agent:

It is responsible for checking communication between AVD service (Broker Service) and VM.

Q Host pool: A host pool is a collection of one or more identical Vmachine or session hosts, that delivers desktop and apps to users.

Azure Bastion: It lets you connect to your virtual machines by secure and seamless RDP and SSH without the need to expose them through Public IP address.

Hybrid AVD Join Deployment Steps:

1. Create a resource group  
2. Under the resource group, Create a Virtual network.  
3. Create a NAT Gateway with a Public IP and associate it with the Vnet.  
4. Create a standalone virtual machine using Windows Server OS.  
5. Start the standalone VM and Connect to it.  
6. Add the ADDS role and promote it to DC.  
7. Install Microsoft Entra Connect a) Installation Configuration Select Agree \> Express setting \> Connect to Entra ID (User name, Global admin Credentials) \> Connect to ADDS using AD user account with who is a member of Domain admin and Enterprise admin.  
8. Create a hostpool, workspace.  
9. Create a VM using Entra AD Credentials.  
10. Create Users on AD, once they are synced. Assign RBAC roles to.  
11. Assign users to Application Assignment.


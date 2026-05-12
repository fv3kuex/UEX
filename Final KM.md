# KM

AVD Km  
1.What is AVD? Explain the connection flow of AVD with diagram.  
Azure Virtual Desktop is a desktop & application virtualization service which runs on Azure.  
AVD Functions:  
 • Full Windows experience \- AVD delivers a full Windows experience with Windows 11 or Windows Server.

 • Single to Multi-Session Compatibility: You can assign a machine to single users by using single session or use multi-session for scalability.

 • RemoteApp: Offer full desktop or use RemoteApp to deliver individual apps.

• Scalability: Scalable and flexible environments could be created.

Here is the connection flow of AVD  in Entra scenario:

1\. Session host maintains permanent outbound connection to RD Broker.

 2\. User requests feed using Windows app or web browser.

   A. User authenticates with Azure AD and gets AAD token.

   B. User presents token to RD Web.

   C. RD Web queries RD Broker (with token) for resources.

   D. RD Broker checks resource directory and geographical databases for .rdp files/icons.

    \* The geographical database contains the connection files (.rdp) and icons for every resource that the user has        been  provisioned .

   E. RD Broker service returns .rdp files to the web service.

   F. RD Web service returns this information to the user's device.

 3\. User requests/initiates connection using the .rdp file.

   A. Remote session begins with a connection to Azure Front Door which provides (global entry point) to AVD.

   B. Azure Front Door directs the connection to the gateway service with lowest latency.

   C. Gateway service queries the broker service for session host with token{AAD}

4\. RD Broker orchestrates the connection between user device and session host.

   A. The Broker service returns the session host to the same gateway service .

5\. Depending upon the configuration and available network protocols , the Connection is made using:

   A. Reverse Connect Transport: After both client and session host connected to the gateway service , it starts relaying the RDP traffic using Transmission control protocol between the client and session host .

   B. RDP Short path: A Direct User Datagram protocol (UDP)-based transport is created between the user's device and the session host bypassing the gateway.

 Reverse connect transport is the default connection type .

2\.  What is host pool? Explain its types? 4

Host pool

A host pool is a collection of Azure virtual machines that are registered to Azure Virtual Desktop as session hosts. All session host virtual machines in a host pool should be sourced from the same image for a consistent user experience. You control the resources published to users through application groups.

 A host pool can be one of two types:

Personal Host Pool: Where each session host is assigned to an individual user. Personal host pools provide dedicated desktops to end-users that optimize environments for performance and data separation.

 Pooled Host Pool: where user sessions can be load balanced to any session host in the host pool. There can be multiple different users on a single session host at the same time. Pooled host pools provide a shared remote experience to end-users, which ensures lower costs and greater efficiency.

3\.  What is application group? Explain its types. 3

An application group is a logical grouping of applications that are available on session hosts in a host pool. 

Application groups control whether a full desktop or which applications from a host pool are available to users to connect to. An application group can only be assigned to a single host pool, but you can assign multiple application groups to the same host pool.

There are two types of Application groups:

 Desktop Application Group: Users access the full Windows desktop from a session host. Available with pooled or personal host pools.

 RemoteApp Application Group: Users access individual applications you select and publish to the application group. Available with pooled host pools only.

4\.  What is the default limit for number of supported application group per AAD tenant?1

500

5\. What is the command to check the status of the sxs listener? 1

Qwinsta

6\.  What are the eligible licenses for AVD? 4

For server : RDS Cals are used for providing the license and a separate licensing server is required to provide the RDS CALs to the machines 

For client ; The AVD client machine  will use the provided license on the machine like E3,E5.

7\. What are the AVD agents? 4

AVD Agents

 AVD agent acts as the intermediate communicator between the AVD service and virtual machine.

 Types of AVD Agents

 i) RD Agent Boot Loader: The agent boot loader is the executable that loads the agent.  
    
Here are few features:

Installs and initializes the RD Agent

Handles updates of the agent

Ensures agent runs correctly during system startup

Acts like a supporting service for the main agent.

 ii) RD Services SXS Network Stack: It is responsible for gateway listeners for incoming connection requests.

Here are few features:

 Traditional Remote Desktop connections required inbound access (port 3389\) to the virtual machine, which posed security risks.

To solve this, Azure Virtual Desktop introduced the SxS network stack, where:

The session host initiates an outbound connection to Azure

Communication happens over secure HTTPS (port 443\)

No direct inbound RDP access is required

The SxS Network Stack Agent plays a key role in enabling this mechanism.

 iii) RD Services Infrastructure Agents: It is responsible for checking communication between AVD service (gateway/broker) to VM.

 Here are few features:

Maintains connectivity with Azure

Supports reverse connect (SxS stack)

Handles session communication

Works in the background to ensure stable connectivity.

   
iv) RD Services Infrastructure Geneva agent: It is responsible for monitoring the health of agents.

 When users connect to Azure Virtual Desktop, many processes happen in the background (authentication, brokering, session creation, etc.).

The Geneva Agent continuously:

Monitors these processes

Collects logs and metrics

Sends data to Azure monitoring systems

This helps in detecting issues, troubleshooting errors, and improving service reliability.

8\. What are the AVD built-in roles. 2

There are four types of AVD built-in roles :  
1\. Owner  
2.Contributor  
3\. Reader  
4.Operator

9\. What are the types of cloud service model? AVD falls under which service model. 2

The cloud refers to a network of remote servers owned and managed by third-party providers, accessible over the internet.

 It provides on-demand access to computing resources like storage, virtual machines, software applications, and networking.

 Types of Cloud Computing service model :

1\. Infrastructure as a Service (IaaS): Users manage their own operating system, software, and data, while the provider manages the infrastructure.

Here are some uses:

 On-demand virtual machines and storage

Scalable resources

Pay-as-you-go pricing

Full control over operating systems and applications

 Examples:

 Virtual servers

Cloud storage

Networking components

 Use Case:

Used by system administrators and IT teams who want flexibility to build and manage their own infrastructure.

 2\. Platform as a Service (PaaS): Users manage their applications while the provider manages the underlying infrastructure and platform tools.

 Development tools and frameworks

Database management systems

Middleware and runtime environments

Automatic updates and maintenance

 Examples:

Application hosting platforms

Web development environments

Use Case:

Ideal for developers who want to focus on coding and application development without managing infrastructure.

 3\. Software as a Service (SaaS): Users access software applications hosted and managed by the provider.

Accessible from anywhere via internet

No installation required

Automatic updates

Subscription-based pricing

 Examples:

Email services

Online collaboration tools

Customer relationship management (CRM) software

 Use Case:

Best for end-users who need ready-to-use applications without technical setup.

AVD falls under Software as service model.

10.What is the difference between RDS and AVD? 4

RDS     

Session-based on virtualization-based deployment           

Session collection     

Published RemoteApps         

Published desktop

Session host role can be installed only on servers               

 AVD

Tenant-based deployment

Host-pool

 Desktop application group

Remote application group

Session host can be installed in clients (single session, multi-session), servers.

   
11.What are the Microsoft managed components in AVD? 4

Managed by Microsoft: 

RD Web service : It is user interface or endpoint where user will be able to see the provisioned resources /icons:

RD Broker : It orchestrates incoming connection 

RD Gateway: It is a websocket for the incoming connection 

 RD Resource directory : It is used to check the resources from the geographical database 

 RD Geographical database: It contains the information like .rdp file and other provisioned resources or icons .

Azure Diagnostics : It is used for diagnosing the issues or to check the health.

12.Which RBAC role is required to configure start VM on connect. 1

Desktop Virtualization Power on Contributor

13\. Mention the PowerShell command to add users to the application group in ARM AVD. 1

new-azroleassignment

14\.  Which AVD agent is responsible for monitoring the health of AVD? 2

Remote Desktop services Infrastructure Geneva agent is responsible for managing the health of AVD .

15\. What are the compulsory RBAC roles for users to connect to AVD? 2

Azure role-based access control (Azure RBAC) helps to manage who has access to Azure resources, what they can do with those resources, and what areas they have access to.

 Azure RBAC is an authorization system built on Azure Resource Manager that provides fine-grained access management to Azure resources.

 1\. Desktop Virtualization User Role

 2\. Virtual Machine User Login Role

16\. What is drain mode in AVD and how do we enable it? 2

Drain Mode: It is a feature in AVD which restricts or prevents the new connections for session host . It is a feature which is highly used when we does not want any connection to land on specific session host , hence we enable drain mode so that no new connection cannot be made.

We can enable the drain mode from Azure portal under Hostpool\>Total Machines

17\. Mention the PowerShell command to create a host pool in ARM AVD.1

new-azwvdhostpool

18.Can I change a host pool from Pooled to Personal?  1

No , We cannot change a host pool from pooled to personal .

19\. Why is targetisaadjoined key required?  1

targetisaadjoined key is required for Entra MFA when connecting in Entra join. This has been replaced by  enablerdsaadauth.

WMI KM

1\.	Give example for WMI      local query and Remote query

Local Query \-Get-WmiObject \-Class Win32\_OperatingSystem

Remote Query \-Get-WmiObject \-Class Win32\_OperatingSystem \-ComputerName "RemotePC" \-Credential (Get-Credential)

Here are few other ways of initiating local query and remote query:

Local query using the webem test tool 

1\. Connect to namespace root\\CIMv2  
2\. Query on webem test select \* from Win32\_BIOS

Remote query using the webem tool :

1\. Connect to namespace of remote computer : \\\\ Computername (remote)\\root\\CIMV2

2\.	Query on Webem Test Select \* from Win32\_BIOS

2.What is the WMI      arbitrator?

WMI arbitrator is a component of WMI service that manages scheduling and execution of tasks received from WMI client process

 It resides in the WMISVChost.exe and is implemented as the following static singleton instance : Wbemcore\! cwmiarbitrator ::m\_Parb

 The arbitrator track queries and incoming client requests as tasks of object type : Wbemcore\!cwmittask().

 Examples of some information tracked for these tasks include Client process id , Client computer name , Last client activity, WQL text query  , Task submission etc.

Results returned by executing these tasks retrieved from WMI provider processes, are held directly in WINMGMT SVChost.exe heap memory until retrieved by clients .

 If the client does not specify WBEM\_FLAG\_FORWARD\_ONLY to indicate a non-rewindable or single direction enumerator, then the complete result set will remain in WINMGMT heap memory for 20 minutes.

3\.	What is a WMI namespace?

WMI namespace is a programming object that  manages the set of classes and objects . It specifically contains all the classes with the different resources defined as object . It contains the root folder and then after the further 

It is a programming object that defines the scope for a set of clases and instances .It is essentially a container or enviroment where WMI provider classers are defined and  managed. Namespaces are used to organize classes that represents different managed environment. Provides a hierarchical organization  of classes.

 A Namespace organizes information similar to folders in a filesystem. The top “folder” of the WMI namespace is always called “root”.

 Default namespace : Root/Default

  Root/CIMv2

4.What is WMI CLASS ?

A WMI class is a template or definition for the WMI objects and the instances. 

Example of WMI classes is as follow:

Win32\_Bios, Win32\_OperatingSystem

5.What are WMI providers?

WMI Providers are intermediary between WMI Service & objects. WMI Providers starts as

a service name WMIPRVSE.exe whenever a query is made.

This acts as a container and provides the requested information to the application/query.

Each time a query is made from a different source, a separate service is started as WMIPRVSE.

It runs in the background and is essential for monitoring, automation, and system management. Disabling it can break many Windows and enterprise features.

The default startup type of WMI service is Automatic.

WMI service depends on RPC.

6\. Which service is      essential for WMI to function?

The RPC is the essential service for WMI to function and also WMI service(winmgmt) has to be running to make the WMI function .

7\. What tools can be used      to query or troubleshoot WMI?

1\. The tool used is WEBEMTest, which allows users to query and interact with WMI data.

2\. WMI Console (Wmimgmt.msc)

4\.	We can use PowerShell tool to test the WMI queries for local and remote queries.

7.How does COM/DCOM relate to WMI?

The following features of COM/DCOM defines how it is related to WMI:

COM : COM is a language-independent,  object-oriented system to create software components in a binary standard which  can interact. COM promotes reuse of software components from different vendors. A COM application usually consists of an executable file and additional components such as dll files. Using COM different components communicate locally with other components within machine environment 

DCOM : It is an extension to COM . It allows access to software components or objects located on a different computer It is used to exchange information between remote computer or a  system located in lan or wan or even on internet.

 Restricted activation and restricted access are  by default given to administrators and are disabled for standard  users

 DCOM Has four aspects of security :

 1\. Access security

2.Launch Security

3\. Identify

5\.	Connectionpolicy

COM and DCOM are essential components for WMI as they act as a communication between, Windows API, WMI infrastructure, and WMI providers.

8\. Where is the WMI      repository located?

C:\\Windows\\System32\\wbem\\Repository

9\. What Windows logs are      used for WMI troubleshooting?

Here are few Windows logs used for WMI troubleshooting : 

1.WMI uses Event Tracing (ETW) and events can be obtained through the Event Viewer user interface or the Wevtutil command line tool. For more information, see Tracing WMI Activity. WMI service activity is recorded in the WMITracing.log file.  
   
WMI uses event tracing to record WMI service activity.

 Event notification support

 Query language support

 Security support

 Scripting access to performance counter data.

The command line tool wevtutil  is used to enable WMI event tracing to locate WMI events.

Here are few other windows logs used :

1\. WMI-Activity Logs

Located in Event Viewer under Applications and Services Logs → Microsoft → Windows → WMI-Activity.

 2.System and Application Logs

Standard System and Application logs can also provide context for WMI issues, especially if errors originate from DCOM permissions, service failures, or firewall 

WMI KM (Original)

What is the command used to launch WMI Management console?  
What is the command used to launch WMI tester tool in Windows Server?  
What is the command used to launch WMI tester tool in Windows Server?  
What is the command to get the WinRM listener configuration ?  
What are the 2 major ports used for WINRM communication?  
What is WinRM? What is WMIPrvse?  
What is MMC ? What is its purpose?  
What is Task scheduler, write down its service name. What are the two types of sessions?  
What is event viewer? What are different types of events found in event viewer? What is the command to launch event viewer?  
What is Event Forwarding in Windows? What are the ways events can be forwarded to the collector? Explain each ?  
What is COM & DCOM ? What is the command to launch DCOM configuration tool? Default permissions for DCOM?  
What are WMI Classes, Instances and Namespaces? WMI Namespaces found by default in Newly Installed OS? Command to repair WMI repository?  
What are 4 basic components in WMI Architecture? Explain WMI Architecture? What is the service name of WMI and what is its startup type?

RDP Shortpath KM 

1.What is RDP shortpath?

RDP Shortpath establishes a UDP-based transport between a local device Windows App or the Remote Desktop app on supported platforms and session host in Azure Virtual Desktop.

 By default, the Remote Desktop Protocol (RDP) begins a TCP-based reverse connect transport, then tries to establish a remote session using UDP.

 If the UDP connection succeeds the TCP connection drops, otherwise the TCP connection is used as a fallback connection mechanism.

1.Managed networks, where direct connectivity is established between the client and the session host when using a private connection, such as Azure ExpressRoute or a site-to-site virtual private network (VPN).

 \* A connection using a managed network is established in one of the following ways:

 \* A direct UDP connection between the client device and session host, where you need to enable the RDP Shortpath listener and allow an inbound port on each session host to accept connections.

 A direct UDP connection between the client device and session host, using the Simple Traversal Underneath NAT (STUN) protocol between a client and session host. Inbound ports on the session host aren't required to be allowed.

   
2.Public networks: where direct connectivity is established between the client and the session host when using a public connection.

 There are two connection types when using a public connection, which are listed here in order of preference:

 \*A direct UDP connection using the Simple Traversal Underneath NAT (STUN) protocol between a client and session host.

 \*An relayed UDP connection using the Traversal Using Relay NAT (TURN) protocol between a client and session host.

 To enable RDP shortpath for managed network you must enable a UDP listener on the session host (on port 3390\) and it can be changed to use a different port.

2.Mention the key benefits of RDP Shortpath

There are several benefits of RDP Shortpath : 

1.Using URCP to enhance UDP achieves the best performance by dynamically learning network parameters and providing the protocol with a rate control mechanism.

 2.Higher throughput.  
   
 3.When using STUN, the removal of extra relay points reduces round-trip time improves connection reliability and     the user experience with latency-sensitive applications and input methods.

4.Higher Efficiency 

6\.	UDP-based transport offers better connection reliability and more consistent latency.

3\. What is MSIX app attach? Mentions few of its features

MSIX app attach is Microsoft's application layering technology, designed for AVD which uses the new MSIX package format to separate applications from the core operating system and deliver applications to user dynamically.

 Applications are not installed locally on Session Host or image (OS image), making it easier to manage custom images for the Session Host, reducing operational overhead and cost for the organizations. Applications run within containers with separate user data, the operating system, and other applications, increasing security and making them easier to troubleshoot.

Features :

1.Removes need of Repackaging: It uses streaming technology to optimize bandwidth by only downloading the difference between an existing package and an upgrade.

2\. Creates Separation: The application lives inside a virtual application container, which ensures strict isolation between the application and the operating system.

3\. Supports Silent Installation: Capturing products is performed similarly to MSI packaging. MSIX supports converting .exe installers and MSI packages unattended.

4\. OS remains unmarked: Once the application is de-staged, no trace file is left within the system (like the application was never there).

 5.Reduces Time: It takes less time for a user to login.

4.Explain staging in MSIX.

Staging involves two steps:

 1\. Mounting the VHD(X)/CIM to the VM.

 2\. Notifying the OS that the MSIX package is available for registration by mounting the apps to the Host pool (Session Host).

5\. Explain the MSIX package structure with diagram

MSIX package contains following structure : 

1.Package Payload: Application files

  a. App Payload: The payload files are the appcode files and assets when building the image (icon).

 2\. Footprint Files:

  \* AppxBlockMap.xml: The package blockmap is an xml file which Contains a list of application files, indexes, and hashes for each block of data stored in a pacakge . Secured with a digital signature.

   
 \* AppxManifest.xml:The pacage manifest Contains info needed to deploy, display, and update the app (identity, dependencies, etc.).

  \* AppxSignature.pfx:  This file is Generated when the package is signed; all packages must be signed before you run them (validation).

\* Code Integrity: MSIX code integrity ensures app security and tamper-protection through cryptographic signatures. It uses AppxSignature.p7x (digital signature), AppxBlockMap.xml (file hashes), and AppxMetadata\\CodeIntegrity.cat (catalog of PE files) to validate that files haven't been altered.

5.Write the steps from where you can enable Start VM on Connect in AVD

We can enable start vm on connect from the azure portal-\>resource group-\>hostpool  or by running the command Update-azwvdhostpool

7\.	Explain de-registration in MSIX  
Deregister: Removes a registered but non-running MSIX package for a user. Occurs during user sign-out; application data specific to the user is pushed to the local user profile. This process is crucial for maintaining system performance and ensuring that the application can be reinstalled later if needed.

8\.	Can MSIX app attach be used with both persistent and non-persistent virtual desktop infrastructures?  
Yes

9\. What is auto scale and how it works?

Auto scale lets you scale your session host Virtual Machines (VMs) in a host pool up or down according to a schedule to optimize deployment costs (by turning session hosts on and off).

Autoscale uses a scaling plan  that defines the schedules for scaling session hosts in a host pool. You can assign one scaling plan to multiple host pools. Each host pool can only have one scaling plan assigned to it.

 A scaling plan can only operate in its configured time zone.

Here are four phases of autoscaling :

Ramp-up : The ramp-up phase of a scaling plan schedule is usually at the beginning of the work day, when users start to sign in and start their sessions. In this phase, the number of active user sessions usually increases at a rapid pace without reaching the maximum number of active sessions for the day yet.

 Peak hours : The peak phase of a scaling plan schedule is when your host pool reaches the maximum number of active user sessions for the day. In this phase, the number of active sessions usually holds steady until the peak phase ends. New active user sessions can be established during this phase, but usually at a slower rate than the ramp-up phase.

 Ramp-down : The ramp-down phase of a scaling plan schedule is usually at the end of the work day, when users start to sign out and end their sessions for the evening. In this phase, the number of active user sessions usually decreases rapidly.

 Off-peak hours : The off-peak phase of the scaling plan schedule is when the host pool usually reaches the minimum number of active user sessions for the day. During this phase, there aren't usually many active users, but you may keep a small amount of resources on to accommodate users who work after the peak and ramp-down phases.

9\.	How many VMs need to be in a host pool for autoscale to work properly?  
Atleast one

Fslogix

What is FSLogix? How is it different from other profile solutions?

FSLogix is a type of user profile solution which enhances and enables a consistent experience for Windows user profiles in virtual desktop computing environments. FSLogix is not limited to virtual desktop environments; it could be used on a physical desktop where a more portable user experience is desired.

Others Profile Solutions                                      

1\. Uses Copy/paste to provide the user profile        

2\. Slower to load user profile.                            

3\. Could be platform limited                                  

4\. Limited scalability                                                   

5\. Office data redirection might be unavailable.    

6\. Concurrency is not supported                         

7\. Could be complicated to configure                   

FSLogix Profile

1\. Uses mount/unmount to dynamically attach the profile container.

2\. Faster to load user profile.

3\. Can be used on any platforms (AVD, RDS, Citrix).

 4\. Highly scalable.

 5\. Office data redirection is available.

6\. Concurrency is supported.

7\. Easy to configure

2\. Mention few features of FSLogix

1\. It provides the roaming profile solution between remote computing session hosts.  
2\. Minimize the login or sign in time in VM environment   
3\. It provides the scalability to run concurrent sessions   
4.It provides the Office redirection feature separately by adding ODFC container  
5.Provides a local profile experience   
6.Simplify the management of apps and gold images.

3\. How many types of container does FSLogix offer? Explain them

FsLogix offers two types of container :

1\. Profile Container : This container is used to manage all the  user profile related data 

This is located at the location HKLM\\Software\\Fslogix\\Profile

A profile container is all the data related to a user's profile, which is directly stored in VHD(X).

 It's a full remote profile solution for non-persistent environments.

 Redirects the entire user profile to the remote location.

 Its configuration defines how and where the profile is redirected.

 The entire profile except "Redirection.XML" is included in the profile container.

 Profile container includes all the benefits of office container.

 Profile container VHD(X) will contain the entire windows profile for the user except for:

The temp folder location

The IE (Internet Explorer) crash folder location

Exclusions are done with a "Redirection.XML" file.

2\. ODFC Container: An ODFC container is a container type which is focused on storing profile content that is unique to Microsoft Office applications.

Office container redirects only areas of the profile that are specific to Microsoft Office apps.

A Default ODFC container includes:  
   
Office Activation  
Outlook  
Outlook personalization  
SharePoint  
OneDrive  
Skype for Business (legacy support)

ODFC containers are an optional configuration.

4.How many types of Profile are available in FSLogix profile container. Explain them

  There are 4 profile types available in FSLogix profile container   
0: Normal profile is provided in this setting.  
1: Read Write permissions are provided in this configuration  
2: Read only permissions are provided in this configuration  
3: It tries to provide the read and write access, but it falls back to read only if failed.

# MSIX

Method 1: Using Group Policy  
Open Group Policy Management: Press Win \+ R, type gpmc.msc, and click OK.  
   
Navigate to Remote Desktop Services: Go to Computer Configuration \-\> Policies \-\> Windows Settings \-\> Security Settings \-\> Local Policies \-\> User Rights Assignment.  
   
Edit RDP Settings: Under Administrative Templates, navigate to Windows Components \-\> Remote Desktop Services \-\> Remote Desktop Session Host \-\> Device and Resource Redirection.  
   
Disable Clipboard Redirection: Find the setting labeled “Do not allow clipboard redirection” and set it to Enabled.  
   
Apply and Exit: Click OK to apply your settings, then close the Group Policy Management Editor.  
   
Method 2: Using the Registry Editor  
Open Registry Editor: Press Win \+ R, type regedit, and click OK.  
   
Navigate to RDP Settings: Go to HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server.  
   
Edit the Clipboard Settings: Find the DWORD value named “fDisableClip”. If it doesn’t exist, right-click and create a new DWORD (32-bit) value and name it fDisableClip.  
   
Set the Value: Set this value to 1 to disable clipboard redirection. To enable it later, change the value back to 0\.  
 

•	You open the RD client (desktop or web) on your local device  
•	All remote sessions begin with a connection to Azure Front Door , which provides the global entry point to AVD. Azure Front Door determines the AVD gateway (RDGateway) service with the lowest latency for your device and directs the connection to it.  
•	The RDGateway service connects to the AVD broker (RDBroker) service in the same Azure region. The RDGateway service enables session hosts to be in any region and still be accessible to users.  
•	The RDBroker service takes over and orchestrates the connection between your device and the session host. The RDBroker service instructs the AVD agent running on the session host to connect to the same RDGateway service that your device has connected through.  
•	The session host connects to the RDGateway (aka Reverse connect).  
•	After both client and session host connected to the RDGateway service, the RDGateway starts relaying the RDP traffic using Transmission Control Protocol (TCP) between the client and session host. Reverse connect transport is the default connection type.  
•	If UDP can be negotiated, a direct User Datagram Protocol (UDP)-based transport is created between your device and the session host, and all traffic is offloaded from TCP to UDP, bypassing the RDGateway service.

Application Masking manages access to Applications, Fonts, and other items based on  
criteria. The Application Rules Editor is used to Describe the item, such as application, to be  
managed. The Editor is also used to define criteria rules are managed by. For instance,  
GitHub should be hidden from the Accounting group.  
Things you can do with the Apps Rules Editor:  
• Create new Rule Sets  
• Edit existing Rule Sets  
• Manage the user and group assignments for Rule Sets  
• Temporarily test rule-sets  
Before using the Application Rules Editor, it must be installed

MSIX app attach process flow.   
1\.	User authenticates against Azure AD and obtains AVD feed.  
2\.	RD Broker working with RD Gateway, and RD agent orchestrate a connection to a VM.  
3\.	VM initiates creation of user session and FSLogix “brings” the user profile.  
4\.	RD Agent is notified what MSIX packages must be registered for the user.  
5\.	RD Agent registers MSIX packages.  
6\.	Session establishment is completed, and user can interact with any of the MSIX packages.  
   
   
.CIM is a new file extension associated with Composite Image Files System (CimFS). Mounting and unmounting CIM files is faster than VHD files. CIM also consumes less CPU and memory than VHD.  
 What is staging?  
MSIX app attach defines staging as an operation that:  
•	mounts an MSIX image from the MSIX file share  
•	parses the content of the MSIX image so relevant OS components can be notified that an MSIX packaged application is available  
•	dependencies are evaluated  
MSIX app attach defines destaging as an operation that:  
•	removes references in the OS to the MSIX packaged application  
•	unmounts an MSIX image from the MSIX file share  
Only MSIX packages with State set to active are staged.  
   
•	Staging happens per session host.  
•	Staging an MSIX package does not grant users permissions to the MSIX application.  
•	When staging fails, for any reason, the session host Status will become Unavailable. The status will remain until the issue blocking staging is resolved or the MSIX package is removed from the host pool.  
•	Staged CIM packages do not appear in the Disk Management tool, only VHD(x) do. Both VHD(x) and CIM files show up when running mountvol.  
   
   
Create an MSIX image for MSIX app attach  
MSLearn Link: Manage MSIX app attach \- Training | Microsoft Learn(opens in a new tab)  
Requirements   
•	Existing AVD deployment  
•	Download the msixmgr tool(opens in a new tab) and save the msixmrg.zip file to a folder within your personal laptop.  
•	Unzip the msixmgr.zipfile and place the MSIX package in the same folder.  
•	Install the MSIXCert.pfx in the Local Machine, under the Trusted Root Certificate store.  
•	Enable Developer Mode under: Settings\>Privacy & Security\> For Developers  
•	Install NotePadPPMSIX\_1.1.0.0\_x64\_\_74gmt6m99dpft.msix  
   
Create an MSIX image for MSIX app attach. Test MSIX app attach by using PowerShell scripts. Publish an MSIX application as a remote app. Maintain MSIX app attach with updates and removals.  
 

WinRM ListnerA management service that implements WS-Management protocol to send and receive messages. WinRM is a listener service. A listener is defined by a transport (HTTP or HTTPS) and an IPv4 or IPv6 address. You can create more than one WinRM listener instance on a single computer by giving them a different TCP/IP address or port number.   
   
   
WINRM   
   
Default Authentication Settings   
The default credentials, user name, and password, are the credentials for the logged-on user account that runs the script.   
   
The following list contains a list of what occurs when a script or application runs under the default credentials:   
•	Kerberos is the default method of authentication when the client is in a domain and the remote destination string is not one of the following: localhost, 127.0.0.1, or \[::1\].   
•	Negotiate is the default method when the client is not in a domain, but the remote destination string is one of the following: localhost, 127.0.0.1, or \[::1\].   
Basic Authentication  
   
To explicitly establish Basic authentication in the call to WSMan.CreateSession, set the WSManFlagUseBasic and WSManFlagCredUserNamePassword flags in the flags parameter. Basic authentication is disabled in the default configuration settings for both the WinRM client and the WinRM server.  
   
Digest Authentication  
   
To explicitly establish Digest authentication in the call to WSMan.CreateSession, set the WSManFlagUseDigest flag in the flags parameter. Digest is not supported. It cannot be configured, for the WinRM server component.  
   
Negotiate Authentication  
   
To explicitly establish Negotiate authentication, also known as Windows Integrated Authentication, in the call to WSMan.CreateSession, set the WSManFlagUseNegotiate flag in the flags parameter.  
   
User Account Control (UAC) affects access to the WinRM service. When Negotiate authentication is used in a workgroup, only the built-in Administrator account can access the service. To allow all accounts in the Administrators group to access the service, set the following registry value:  
   
HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Policies\\System\\LocalAccountTokenFilterPolicy = 1  
   
Kerberos Authentication   
To explicitly establish Kerberos authentication in the call to WSMan.CreateSession, set the WSManFlagUseKerberos flag in the flags parameter. Both the client and the server computers must be joined to a domain. If you use Kerberos as the authentication method, you cannot use an IP address in the call to WSMan.CreateSession or IWSMan::CreateSession.   
   
Client Certificate-based Authentication   
To establish client certificate-based authentication in the call to WSMan.CreateSession, set the WSManFlagUseClientCertificate flag in the flags parameter.   
You must first enable certificate authentication on both the client and service by using the Winrm command line tool.  
   
Collector Initiated   
•	With a Collector Initiated subscription the collector will have to establish outbound WinRM connection and Pull events from the clients.   
•	It has the following requirements:    
o	The WinRM service has to be enabled on the client.  
This service is enabled by the default on the server operating systems but it is disabled by default on client operating systems  
o	The client has to be reachable over the network and a firewall rule needs to be configured to accept the incoming traffic   
o	The collector will have to contact each clients to pull the events, for this reason this solution is not really scalable and it is not the preferred option   
   
Source Initiated   
•	With a Source Initiated subscription the collector will wait for incoming connection coming from the clients.   
•	It has the following requirements:    
o	The collector has to be reachable by the clients   
o	A policy has to be deployed to the clients to point them to the collector   
   
 

# Rds & AVD

Rds

RD Virtualization Host : RD Virtualization Host is a Remote Desktop Services role service. RD Virtualization Host integrates with Hyper-V to provide virtual machines that can be used as personal virtual desktops or virtual desktop pools. An RD Virtualization Host server has the following functions: \- Monitoring virtual machine guest sessions and reporting these sessions to the RD Connection Broker server. \- Preparing the virtual machine for a remote desktop connection when requested by the RD Connection Broker server.

AVD

Authentication in AVD  
When accessing AVD, there are 3 authentication phases:  
•	Cloud service authentication  
•	Remote session authentication  
•	In-session authentication  
Troubleshooting an authentication issue during any of these 3 phases, requires the same steps as troubleshooting the same phase on an Azure VM or on-prem device.  
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  
Authenticating to the AVD service (Cloud service authentication)  
AVD supports different types of identities depending on which configuration you choose: On-premises identity, Hybrid identity, Cloud-only identity, Federated identity, External identity.  
To access AVD resources, you must first authenticate to the service by signing in with a Microsoft Entra ID account. Authentication happens whenever you subscribe to retrieve your resources, connect to the gateway when launching a connection or when sending diagnostic information to the service. The Microsoft Entra ID resource used for this authentication is Azure Virtual Desktop (app ID 9cdead84-a844-4324-93f2-b2e6bb768d07).  
You can also make use of MFA, Passwordless authentication, Smart card authentication.  
For more details, see: Cloud service authentication   
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  
Authenticating to an AVD VM (Remote session authentication)  
Authenticating on an AVD VM works exactly the same as when connecting via RDP to any other Azure VM.  
You can also make use of Single Sign-On (SSO).  
Azure Virtual Desktop supports both NT LAN Manager (NTLM) and Kerberos for session host authentication, however Smart card and Windows Hello for Business can only use Kerberos to sign in. To use Kerberos, the client needs to get Kerberos security tickets from a Key Distribution Center (KDC) service running on a domain controller. To get tickets, the client needs a direct networking line-of-sight to the domain controller. You can get a line-of-sight by connecting directly within your corporate network, using a VPN connection or setting up a KDC Proxy server .  
For more details, see: Remote session authentication   
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  
In-session authentication  
This is related to authenticating to applications and web sites within the remote session.  
Here it is important to understand better the type of application used and its authentication requirements, as each may require a different approach.  
RDP can provide credential delegation inside the session, to some degree, but different security settings, outside the control of AVD, may also prevent this from happening.  
AVD in-session passwordless authentication using Windows Hello for Business  or security devices like FIDO keys when using the Windows Desktop client .  
For in-session use of smart cards, you need to install the smart card drivers also on the session host and enable smart card redirection.  
For more details, see: In-session authentication 

Azure Avd Framework   
In the Azure Virtual Desktop Service framework, there are three main components:  
•	AVD Service  
•	Remote Desktop Client  
•	Virtual Machine  
On a Virtual Machine, we have:  
•	AVD Agent  
o	AVD Agent Bootloader  
•	SxS Stack  
•	Geneva Monitoring Agent

RDP gateway 

The Gateway service (RD Gateway) is a websocket service that provides the Remote Desktop Protocol (RDP) connectivity from a user's device wherever they're connecting from to the session hosts providing their desktops and apps.  
The RD Gateway securely connects users to their sessions over the Internet.  
Aside from the session hosts themselves, depending if RDP Shortpath (UDP) is being used or not, the RD Gateway may or may not be the only other role used throughout the lifespan of a session.  
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  
When a user connects to a desktop or app from their feed, the RDP connection is established as follows:  
1\.	All remote sessions begin with a connection to Azure Front Door, which provides the global entry point to Azure Virtual Desktop. Azure Front Door determines the Azure Virtual Desktop gateway service with the lowest latency for the user's device and directs the connection to it  
2\.	The gateway service connects to the broker service in the same Azure region. The gateway service enables session hosts to be in any region and still be accessible to users.  
3\.	The broker service takes over and orchestrates the connection between the user's device and the session host. The broker service instructs the Azure Virtual Desktop agent running on the session host to connect to the same gateway service that the user's device has connected through.  
4\.	At this point, one of two connection types is made, depending on the configuration and available network protocols:  
a. Reverse connect transport: after both client and session host connected to the gateway service, it starts relaying the RDP traffic using Transmission Control Protocol (TCP) between the client and session host. Reverse connect transport is the default connection type.  
b. RDP Shortpath: a direct User Datagram Protocol (UDP)-based transport is created between the user's device and the session host, bypassing the gateway service.

RDP diagnostic  
The Diagnostics service (RD Diagnostics) is an event-based aggregator that marks each user action on the AVD deployment (end-user or administrator) as a success or failure. Administrators can query the aggregation of events to more easily identify a failing component.  
In AVD we leverage our diagnostic role to troubleshoot failures. We can get everything except network.  
•	AVD logs a diagnostic activity any time a user interacts with the system. This includes connection process, roles involved in the transaction, error messages, tenant information, and user information  
•	The diagnostic service aggregates activity information based on events generated by all AVD service components: RDP clients, service roles (Rdweb, Gateway, Broker), service components from customer VM (RD Agent, SxS stack)  
•	All this data is sent to AVD Kusto database where can be reviewed  
The diagnostics role service will cover the following scenarios:  
•	Connection activities: A connection activity is triggered from the end user using one of our apps that support AVD (see client guidance for more information). On failed activities use the user UPN to identify at which component the connection didn't succeed.  
•	Management activities: A management activity is triggered by the infrastructure administrator configuring the system. On failed activities use the UPN to identify what action caused the issue.  
Diagnostics Data Flow  
•	Remote Desktop client sends connection and feed subscription information  
•	RD Session Host agent sends additional information on the connection and health information  
•	This data travels through the system where the roles that are part of that interaction enhance that data by additional information which the client doesn’t has or might be no accurate.  
•	On the named interaction one or more components send data to the diagnostics service which correlates the data and stores it in the SQL database.

Agent Bootloader  
The AVD agent bootloader is the executable that loads the AVD agent.  
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  
Agent Heartbeat  
There are 2 heartbeats in AVD  
1\.	Agent Heartbeat to the broker is to make sure the VM is alive and ready to do orchestration  
2\.	Stack Heartbeat to the client through the wvd gateway is for the client to be able to detect if the connection is lost  
The AVD agent sends a heartbeat to the AVD service (Broker) every 30 seconds.  
This is to make sure that the VM is alive, ready to do orchestration, and report health status.  
If the Broker misses a number of heartbeats from the AVD agent, it will mark the AVD VM as unhealthy and stop sending new connections to it until the underlying issue is resolved and heartbeats are being received successfully again.  
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_  
RDSAGENT.WVD certificates  
These are certificates that the Agent uses to connect to Geneva backend over HTTPS. There is code in the Agent to delete the expired certs.  
SXS network stack   
The SxS Stack on an AVD VM sends packets to the client every 1 second which is the heartbeat, and the client watches that. If the client misses 8 of those packets, it enters in a state of concern (warning state), if it misses 12 more, the client is going to declare that the connection is down and disconnect then try to reconnect.

Printing 

A page description language (PDL) is a language that describes the appearance of a printed page in a higher level than an actual output bitmap.

 PCL  
Printer Command Language, more commonly referred to as PCL, is a page description language (PDL) developed by Hewlett-Packard as a printer protocol and has become a de facto industry standard. Originally developed for early inkjet printers in 1984, PCL has been released in varying levels for thermal, matrix, and page printers. HP-GL/2 and PJL are supported by later versions of PCL.  
PS  
PostScript (PS) is a page description language in the electronic publishing and desktop publishing business.   
XPS  
XML Paper Specification (XPS): An XML-based document format. XML Paper Specification (XPS) specifies the set of conventions for the use of XML and other widely available technologies to describe the content and appearance of paginated documents.   
EMF Data Type   
Enhanced Metafile (EMF) data consists of instructions to call GDI functions. The print processor must call the GDI functions to render printable images. The GDI functions make calls to the printer driver's printer graphics DLL(opens in a new tab), which renders the image and sends it to the spooler as RAW data (by calling EngWritePrinter(opens in a new tab)).   
XPS Printing Features   
The XPS print path and the XPSDrv printer drivers offer the following advantages over the GDI-based printer drivers in the GDI print path:   
•	Improved Color Printing(opens in a new tab)  
•	High-Fidelity Print Output(opens in a new tab)  
•	Improved End-User Experience(opens in a new tab)   
•	Improved Spooling and Rendering(opens in a new tab)  
•	Extensible Architecture  
•	RAW Data Type   
•	RAW data can be sent to a print monitor without further processing. The print processor just sends this data back to the spooler (by calling WritePrinter, described in the Microsoft Windows SDK documentation), sometimes inserting form feeds. An example of a RAW data file is one consisting of printer control language (PCL) commands. Print jobs are sent from client to server in RAW format if either the client or the server does not support NT-based-operating system EMF, or if a server administrator has disabled EMF support.

SPLWOW64.EXE process allows 32-bit applications running on a 64-bit Windows to interact with the 64-bit print drivers that would normally be loaded into the process during printing (Print Preview, Print UI Options, etc.).

If the print queue is set to disable Advanced Options (which disables using EMF spooling), then the print job will be rendered under the SPLWOW64.exe process.

WSD  
WSD is a protocol for communicating with a device (in this case a networked scanner) over the network   
WSD allows network-connected IP-based devices to advertise their functionality and offer these services to clients by using the Web Services protocol.  
WSD-based devices and clients communicate over the network using a series of SOAP (Simple Object Access Protocol) messages over UDP and HTTP(S).  
WSD provides a network plug-and-play experience that is similar to installing a USB device.  
Web Services on Devices (WSD) extends the existing Web services architecture to resource-constrained devices, such as PDAs, computer peripherals, computing appliances, and consumer electronics. WSD is one of four technologies that comprise the Network Connected Device (NCD). Those four are:   
•	PnP-X — a set of extensions to Plug and Play (PnP) that integrate NCDs into the Windows PnP subsystem, making the NCD appear as a local device inside of Windows and providing an installation experience that is similar to attaching a physically connected device.   
•	Function Discovery (FD) — a new API makes it easy for applications to enumerate and use devices of a specific type, regardless of the way in which they are connected to the computer. Function Discovery works through providers for the various device types to enumerate available devices (for example, PnP, registry, WSD, SSDP, NetBIOS, and third party providers).   
•	WSD — an implementation of the Device Profile for Web Services (DPWS) specification that enables devices to interact with Microsoft Windows over an IP-based network. The WSD functionality is part of the Rally licensing program (http://www.microsoft.com/rally/). The DPWS specification is available free of charge. This specification is under the licensing terms of Rally.   
•	Publication Services — a new Windows service that enables client applications to publish resources so that they can be discovered by other computers on the same subnet. Each application can dynamically register and unregister individual resources that it wants published. 

Client-Side Rendering Components   
Client-side rendering functionality is implemented in win32spl.dll, the network print provider.   
The offline printing functionality is implemented in printcom.dll, which provides online/offline notifications to win32spl.dll.   
The client-side rendering components, printcom.dll and win32spl.dll, are both loaded in the Print Spooler service, spoolsv.exe, when it is started.   
Disabling Client-Side Rendering    
Client-side rendering is enabled by default on Windows Vista and Windows Server 2008 and is controlled by the print server that hosts the shared printer. Client-side rendering may be disabled via Group Policy or on the Sharing tab of the properties for specific printers on the print server

Printer Pruning   
When a printer is deleted from a print server, the corresponding Active Directory object is removed. However, there might be times when a printer is temporarily unavailable, such as when a print server is being rebuilt or is powered off. Under these circumstances, you must remove the orphaned entries removed because Active Directory must reflect only the printers that are currently available. 

A program called the printer orphan pruner helps you remove the orphaned entries by checking each domain controller for orphaned printers. By default, if the pruner cannot see a printer three times in a row at 8-hour intervals, it assumes that the entry is no longer valid and deletes it.  
Print Defaults – These are global settings for the printer. When a printer is installed via Point and Print, the Print Defaults are downloaded and configured on the client.    
Print Preferences – These are settings that a specific to the user and are stored in the user's profile. When printing, the user's devmode is validated by the driver to confirm that the devmode is compatible. In some cases with WWAN and Branch Office Printing, having Print preferences can cause a delay, as the client may need to contact the print server over the WAN to validate the devmode.  

IPP (Internet Printing Protocol) is a protocol that allows printing using HTTP, which theoretically allows printing from anywhere.   
On Windows Server IPP is implemented on print services and relies on the IIS web server.   
The installation of Internet Printing on Windows Server sets up a self-service web portal that allows the user to connect a printer to their workstation (Internet Explorer only) and manage print queues.

Printer Redirection is the feature that allows a local printer to be mapped on a remote machine, and allows printing across the network or Internet.  
Printer redirection was first implemented in Windows 2000 Server. Printer redirection enables the users to print to their locally installed printer from a terminal services session. The Terminal Server client enumerates the local print queues to detect the locally installed printers. This list is presented to the server and server creates the print queue in the session.  
Group Policy   
Computer Configuration\>Administrative Templates\>Windows Components\>Remote Desktop Services\>Remote Desktop Session Host\> Do not allow client printer redirection /Disable

TSSV2  
When you encounter printing issues on Windows, it can be challenging to identify the root cause. There can be various reasons why a printer might not work correctly, including driver issues, network connectivity problems, or configuration issues. In such situations, it can be helpful to collect diagnostic data to troubleshoot the problem effectively. Microsoft provides a script called TSSv2.ps1, which collects the necessary data to help you identify and fix printing issues.

Print BRM  
PrintBRM is a command-line tool used to manage printer configurations on Windows servers, allowing for backup, restoration, and migration of printer settings, ports, and drivers.

W365

Windows 365 Enterprise is a cloud-based service that creates Windows VMs (Cloud PCs) for users, providing the benefits of Microsoft 365\. It uses Microsoft Intune for management, Microsoft Entra ID for identity and access control, and Azure Virtual Desktop (AVD) for remote connectivity.  
With the Windows 365 service, you can also:  
•	Configure provisioning policies to create custom Cloud PC configurations.  
•	Customize your Cloud PC configurations to support different user needs.  
•	Pre-install apps in your custom Cloud PC image and push more apps to them through Microsoft Intune.  
Windows 365 dedicated Cloud PC	Windows 365 Frontline Cloud PC  
A dedicated Cloud PC can be assigned and licensed to only one user.	A shared Cloud PC is a resource designed for specific purposes that can be allocated and licensed to multiple users.  
License is assigned to a user.	License is not assigned to individual users but rather at the tenant level.  
For each license, you can provision one CPC for one user.	For each license, you provision three Cloud PCs.  
When a customer purchases 100 licenses, they are entitled to provision 100 CPs.	When a customer purchases 100 licenses, they are able to provision up to 300 Cloud PCs. However, only 100 of these can be active simultaneously.  
Dedicated Cloud PCs are always operational.	Shared Cloud PCs are powered off when a user disconnects from the session, making the virtual machine available for another user.

# WMI

WMI   
Lewis Liu  
Microsoft Server Core Team

Agenda  
1\. WMI basic  
2\. WMI T-Shooting  
3\. WMI advance  
What is WMI?  
• WMI is the Microsoft implementation of the Common Information   
Model (CIM) initiative developed by the Distributed Management   
Task Force (DMTF).   
• In order to unify the management techniques for the sake of   
simplicity, the DMTF defined CIM to represent real-world   
manageable entities in a unified way.   
• The CIM object model is an object data model using terms and   
semantic that is unique to all constructors and software   
developers. This object model is implemented in a database called   
the CIM repository.

Who need WMI?  
• Automation of IT Admin Tasks  
• Health Monitoring  
• Inventory  
• Troubleshooting

WMI Architecture  
WMI Consumers  
(Management  
Applications)  
WS-Mgmt.  
C/C++  
Client  
WMI Listener  
For WS-Mgmt  
3  
.NET Client Applications  
Windows Forms  
Web Forms  
Scripts  
WMI Scripting API  
System Management  
6  
COM Inter-Op  
WMI COM API  
COM/DCOM  
5  
4  
WMI Infrastructure  
COM/DCOM  
WMI  
Repository  
WMI Core  
(CIM Object Manager)  
3  
2  
1  
COM Inter-Op  
SNMP WMI   
Provider  
WMI   
Provider  
s  
Managed  
Objects  
SNMP   
Managed entity  
Native C/C++  
Cimv2 WMI   
Provider  
Windows (Win32)  
Managed entity  
Any WMI   
Provider  
…  
Any   
Managed entity  
(Native Code)  
2  
1  
System.Management  
(Instrumentation Objs)  
.NET WMI Provider  
.NET Managed   
App/entity  
.NET C\#, VB.NET, etc  
CIM OBJECT MANAGER  
• CIM: Common Information Model  
• A component in the WMI infrastructure that handles the   
interaction between management applications  and providers.   
• The CIM Object Manager supports services such as event   
notification, remote access, and query processing.   
• The CIM Object Manager also grants access to the WMI   
repository.

WMI CLASS  
• WMI class is a template for a type of managed object.  It   
represents that entity in terms of the data, methods, and   
properties exposed by that entity  
• WMI classes define the basic units of management.   
• For example, the Win32\_LogicalDisk class is a template for all   
logical disks that occur in the Windows environment. WMI can use   
the class template to generate an instance of Win32\_LogicalDisk   
for each disk installed on a computer.  
WMI Classes  
http://msdn.microsoft.com/en-us/library/aa394554(VS.85).aspx

WMI PROVIDER  
• A COM server that communicates with managed objects to access   
data and event notifications from a variety of sources, such as the   
system registry or an SNMP device.   
• Providers forward the information to the CIM Object Manager for   
integration and interpretation.

WMI PROVIDER SETUP  
Mofcomp.exe  
(compiler)  
REGSVR32.EX  
E  
MOF  
Repository

DLL  
DCOM  
Provider: Mof and dll  
.MOF file   
• An ascii file which contains the class definitions and the provider that   
need to be registered into the repository.    
• Mofcomp.exe compiles the MOF file which registers it into the WMI   
repository.  Once compiled, the classes, namespace, and provider are   
registered in the WMI repository.   
• By reading the repository registrations, the WMI service knows which   
provider will handle the request for the Provider   
.dll File  
• A DCOM object which interacts with the manageable entity and   
abstracts the native API’s of the manageable entity from the WMI   
service.    
• As the WMI service acts a broker between the WMI clients and provider,   
the provider dll’s act as a broker between the manageable entities and   
the WMI service.  

• Regsvr32.exe is the tool used to register the provider .dll with DCOM.  
WMI Processing Flow  
Repository Namespaces  
ROOT  
Select \* from Win32\_Process  
WMI   
Client \#1  
WMI SVC  
DEFAULT  
CIMV2  
RPC Thread   
\#1  
RPC Thread   
\#2  
CORE QUEUE  
WMI   
Client \#2  
Req \#1  
Req \#2  
SMS  
CoreQueue Thread \#1  
Select \* from CCM\_Client  
RSOP  
CoreQueue Thread \#2

WMIPRVSE.EXE  
CIMWIN3  
2.DLL  
Win32\_Process   
Class Definition  
WMIPRVSE.EXE  
SMSCLIE  
NT.DLL  
WMI Object Model relationships  
Simplified WMI Object Model relationships between the WMI locator,   
service, properties, methods, qualifiers, and other objects 

The elements of WMI Object Model \-1  
• Locator Used to connect to the WMI service on a computer.  
• Service Used to connect to the WMI service on a computer and is   
the main point of contact to WMI for programs.  
• Objects Fundamental representations of computer elements and   
are used by WMI and your scripts to identify to providers which   
specific elements that you want manipulated.  
• Events Changes to WMI objects. Events can be captured as   
objects and then manipulated in the same ways as any other   
objects, except that they cannot be changed or saved in WMI.

The elements of WMI Object Model \-2  
• Properties Supplies descriptive or operational information about   
an object.   
• Methods Actions that you can execute on objects.   
• Qualifiers Characteristics of objects, properties, and methods. For   
example, a qualifier for a property might indicate that it is read  
only, or it might list the allowable values for the property. A   
qualifier for an object might be that it is read-only.

WMI Security  
•Golden rule: WMI is a broker, it doesn’t do   
anything that the client is not allowed to do.  
Leverages impersonation  
Expects the manageable entity to validate the access  
•WMI actions are subject to access checks at the   
namespace level  
Read access  
Method execution  
Write Operations  
Remote Access  
Security Changes (DACL)

WMI Dependencies  
• DCOM/COM, Registry, Filesystem, Repository and RPC are   
the basic underlying dependencies of WMI.   
• WMI provides one common interface that is built on DCOM.  
• A Provider installation requires a provider .dll and a .mof file.

WMI repository  
What is the Repository?  
•-By default it is a place where Class information is kept as a   
reference for the OS  
What is a Normal Repository size?  
• 10-50M Objects.data file  
How can you see what's in the repository?  
• You need to open the objects.data file in a hex editor like ultra   
edit  
• The data in the repository is not in chronological order

Files in Repository folder  
• OBJECTS.DATA      \- The page-based data file. It has all the   
repository objects indexed by hash codes.   
• INDEX.BTR            \- The page-based BTree index file. It has all the   
indexes (hash codes) for the objects in OBJECTS.DATA.  
• MAPPING.VER         
 \- The version file. It saves the information to   
tell WMI service which mapping file below has the latest mapping.  
• MAPPING1.MAP      \- The mapping file A. It has the mapping from   
each transactional logical page ID to physical page ID.   
• MAPPING2.MAP      \- The mapping file B. Same as   
MAPPING1.MAP

WMI repository corruption  
• The Repository corruption is EXTREMELY difficult to try and track   
down. Some common causes for corruption are improper   
shutdowns of machines and 3rd party filter drivers that are   
having issues. 

WMI repository Rebuild  
One of the most encountered WMI namespace corruption error is   
0x8004100E  
run winmgmt /resetrepository  
• Side effect:  
1\. All 3rd party WMI namespace will be deleted.   
2\. The rebuild only restores default OS WMI namespace.  
3\. Some Application need to be reinstalled to recovery their WMI   
namespace.  
Note:  
• Rebuild the WMI repository should always be the last option for t-Shooting   
WMI issue.    
• A Backup operation of the old repository is required.

Manually rebuild the WMI repository  
1\. Open A CMD Prompt  
2\. wmimgmt /resetrepository  
3\. Cd windows\\system32\\wbem  
4\. for /f %s in ('dir /b /s \*.dll') do regsvr32 /s %s  
5\. cd /d c:\\  
6\. for /f %s in ('dir /s /b \*.mof \*.mfl') do mofcomp %s

Buffer thresholds  
• The WMI core gathers the data in a buffer until it can be delivered   
to the client. If the client is slow or fails to respond, WMI will   
continue to hold on to the data until the internal buffers reach the   
defined threshold limits.   
• When these threshold limits are reached the WMI service is   
considered to be under stress. In this situation the service slows   
down  the responsiveness of the providers by injecting sleep   
delays. This is done to give the WMI client(s) a chance to catch up.   
• Depending on how much stress the service is under these delays   
could be as large as 60 seconds.  
• The buffer threshold key is:  
HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\WBEM\\CIMOM  
"Low Threshold On Events (B)"="1000000"  
"High Threshold On Events (B)"="2000000"

Characteristics   
1\. Repository and security changes  
2\. WPP Tracing  
3\. Event Log Tracing  
4\. Debugging Options for nasty WMI errors  
5\. New WMI Service switches

Enhancements  
1\. WMI Namespace security  
•Auditing  
•Security settings configurable via scripts and MOF files   
2\. WMI Tracing  
•Windows software trace preprocessor (WPP)  
•WMI-Activity Event Log (ETW)   
3\. CIM repository enhancements  
•Addressing repository corruption issues  
•WMI Migration 

WMI provider enhancements \- 1  
Existing Providers:  
• Scripting of printers, services, reg keys, Dcom apps, and WMI   
namespace security.  
• You can also script the security of Win32 Services.  They do not   
have to be admin or power users now.  
• You can grant a plain user the ability to stop and start a   
service.   
• Hardware and software inventory 

WMI provider enhancements \- 2  
New Providers:  
• Boot management (no Boot.ini anymore)  
• Trusted Platform Module (TPM)  
• Volume Encryption  
• DFS Configuration and monitoring  
• Software licensing

WMI provider enhancements \- 3  
Difference:  
• Performance counters \- the model has been changed due to   
arch issues.  the way the counters are exposed have changed   
under the covers to make them more reliable.  No more   
synchronization is needed.   
• Windows Parental control \- you can prevent access to   
certain areas of the OS and prevent internet access.   
• Terminal Server \- active sessions can be managed, you can   
through WMI force a disconnect of an active session

Performance counter  
Wmiadap  
http://msdn.microsoft.com/en-us/library/aa394528(VS.85).aspx  
• Beginning with Windows Vista, the transfer of data from the   
performance libraries and the refresh of classes derived from   
Win32\_PerfRawData and Win32\_PerfFormattedData is done   
by the WmiPerfClass and WMIPerfInst providers and does not   
require the AutoDiscovery/AutoPurge (ADAP) process.   
• The WmiPerfClass provider updates WMI Performance Counter   
Classes only when a new performance object is added.

WMI Tracing  
• No more WMI logs. Now we have WPP trace preprocessor (WPP)  
• Event Tracing for Windows (ETW), displayed in the Windows Event   
Log Viewer. But by default, activity reporting is disabled.  
Description on how to turn on WMI Event Tracing.  
http://msdn2.microsoft.com/en-us/library/aa826686.aspx

WPP Basics  
WPP is for software tracing  
Its a light-weight standard tracing facility  
It’s a layer on top of ETW  
There are trace providers and trace consumers  
Tracing is always available and activated on demand.  
Easy to implement and supports Windows 2000   
Traces can be viewed using the debugger  
Fine grain tracing control, using level, flags, and can handle   
application conditions

WMI Service switches  
Winmgmt  
1\. /backup  
2\. /restore  
3\. /resyncperf  
4\. /standalone  
5\. /sharedhost  
6\. /verifyrepository  
7\. /salvagerepository  
8\. /resetrepository

Part 2:  WMI T-shooting

General T-Shooting Methods  
•General T-Shooting Tools:  
1: dump / user dump  
2: TTTracer  
3: Event Log  
4: Perfmon  
5: Netmon  
•Next: Focus on WMI t-Shooting Tools

WMI ERRORS Code  
• 0x800410xx and 0x800440xx are WMI errors. It means that a   
specific WMI operation failed. For instance, it could be due   
insufficient privileges to perform the WMI requested operation or   
due to the nature of the request itself or due to a WMI   
infrastructure issue, such as WMI DCOM registration issue.  
• 0x8007xxxx are Win32 errors, not WMI errors. WMI may return   
these types of error due to an external failure, for example, DCOM   
security.  
• 0x80040xxx are pure DCOM errors, not WMI errors. WMI may   
return these types of error due to an external failure (for example   
DCOM configuration).

• 0x80005xxx are ADSI errors, not WMI errors. WMI may return   
these types of errors due to an external failure, such as an Active   
• 0x80005xxx are ADSI errors, not WMI errors. WMI may return   
these types of errors due to an external failure, such as an Active   
What do “Access Denied” errors   
mean?  
• 0x80041003 (WBEM\_E\_ACCESS\_DENIED)   
This typically results when the process / Specific user trying to   
access the namespace does not have the required WMI privileges.   
• Check WMIMGMT.EXE and permissions for that namespace.  
• 0x80070005 (DCOM ACCESS\_DENIED)  
This error occurs when the connected user is not recognized or is   
restricted in some fashion by the remote server  
• The username/password does not exist.   
• The user does not have the remote launch or remote activation   
options set.    
• The specific user does not have the DCOM permissions..  
• 0x800706xx (DCOM RPC error)  
This often occurs when a firewall is configured on the remote   
computer. 

Typical WMI Issues  
Configuration   
Issues  
Dcom Security  
Firewall Config.  
WMI Namespace   
Security

Infrastructure   
Issues  
WMI Service   
Startup  
DCOM Registration  
Missing WMI Class  
Improper WMI   
Provider   
Registration  
Missing System   
Files  
WMI Repository  
Deleted WMI   
Repository  
Typical WMI issue Symptom and   
Cause  
Symptom  
Slow boot/Slow logon  
Cause  
Large Repository size  
Excessive inventorying  
Fast/Excessive Repository   
growth  
Inventorying from System   
Center  
Third party providers  
RSOP logging  
Local WMI queries  
Missing providers  
Permissions in Registry  
Permissions on files  
DCOM  
Remote WMI queries  
DCOM  
RPC errors  
Firewall  
High CPU in WMIprvse /   
SVCHost  
Stuck Queries  
Bad queries  
Excessive queries

Find the WMI provider in   
WMIPrvse.exe  
• tasklist /m |findstr /i "wmi“

Find the PID  
For svchost.exe (Winmgmt):  
1: tasklist /svc  
2: when WMI doesn’t working, try sc queryex  
For WMIPrvse.exe  
1: check the dll for the class  
in MSDN:  
For example, win32\_process  \-\>  cimvin32.dll  
2: tasklist /m cimwin32.dll  
This can be used when capturing the TTT log.

Some of WMI Tools  
• Event Log Tracing for Win 2k8 or later  
• Informational Events Only.  
• Client PID, namespaces, queries, etc.  
• ETW Tracing  
• WMI Tracer  
• WMIDiag  
• So far it is supporting Win 2003\.  
• On 2008, it will report 0x80041002 \- (WBEM\_E\_NOT\_FOUND)   
errors  
• Winmgmt.exe  
• Good for overall health of WMI.

• Backup/Restore repository.  
• Move WMI service to a separated / shared svchost.exe  
System WMI tools \- 1  
WMImgmt.msc  
• The quickest way to   
test the WMI service :   
start the   
Wmimgmt.msc,  and   
check the properties   
on "WMI Control   
(Local)"  
Check namespace   
security   
Check remote WMI   
connection  
Backup / restore WMI   
repository

System WMI tools \-2  
WBEMTEST.exe  
• Low level tool for browsing   
WMI Schema  
• List classes, instances,   
execute methods, performs   
WQL queries (WMI Query   
Language)  
• First line of defense when   
troubleshooting WMI   
related problems-script or   
otherwise

System WMI tools \-3  
DCOMCNFG.exe  
• WMI is built upon Dcom  
• Dcomcnfg.exe to check DCOM settings

WMI-Activity log   
• Event Log Tracing – Beginning in Vista and Windows Server 2008,   
there is a built-in WMI tracing mechanism located in the Event   
Viewer which will log detailed information about WMI activity.  The   
events captured in this trace are “INFORMATIONAL ONLY” –   
therefore, you will not see any errors logged in this trace view.    
• However, this tracing facility will provide you with information   
about CLIENT PID’s, Machine Name’s, Namespaces, the   
Query text, and Start/Stop time.  Start/Stop time might be   
good to know if the customer is complaining about the turnaround   
time for a typical query or other WMI operation.

WMI-Activity Log \- Enable  
How to enable WMI activity event log  
1\. Right click on My Computer, select “Manage”. Expand “Event   
Viewer”  
2\. Expand “Application and Services Logs” – note: this may take a few   
moments.  
3\. Expand “Microsoft” and the “Windows” folder.  
4\. Click on the “Microsoft” folder and then click on the “View” menu   
and click on “Show Analytic and Debug Logs”.  The folders may blink   
for a minute.  
5\. Scroll down until you see the WMI-Activity folder and expand it.  
6\. Right click on the “Trace” icon and select Properties.  
7\. Check the “Enable Logging” button.  You will be prompted to clear   
the existing logs.  
8\. Click OK.  Then to view the traces, click on the “Trace” icon.  
9\. Notice, the logs that are generated. 

• command:  
• command:  
WMI-Activity log properties

Repository Auto Recover Feature: Win   
2k8  
Reg key:  
HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\WBEM\\CIMOM  
• Autorecover MOFs    
• AutoRestoreEnabled  
• Repository Directory

Separate the WMI Service  
The core of WMI is hosted in service host.   
We can move it to a dedicated SVChost.exe  
Winmgmt /standalonehost and /sharedhost  
NOTE: Moving the core into it’s own host can be helpful to isolate   
CPU and Memory spikes 

WPP trace  
ETW Trace tool: WMITrace.exe  
Download:  
http://toolbox/wmitracer   
Usage:   
1\. Repro the issue and collect the ETW log.  
2\. Parse the ETW log by TMF (trace message format )   
convertor

WMIDiag  
1\. Download WMI Diagnosis Utility \-- Version 2.0 self-extracting zip   
file.  
http://www.microsoft.com/download/en/details.aspx?displaylang=en\&id=768  
4  
2\. Open a command prompt window and navigate to the extracted   
files, run:  
cscript WMIDiag.vbs    
3\. This may take quite a few minutes to complete. Once completed   
you will have the following file:  
%TEMP%\\WMIDIAG\*\*\*\*\*.CSV \\ .TXT \\.LOG

4\. Most of the info can be ignored, but we should open the log file   
and search for the WMI REPORT: BEGIN section of the file. The   
report section provides a summary of the tests run by the tool.  
Associating Classes, Providers,   
and MOF Files in WMIDiag  
One very useful function included in WMIDiag is   
the ability to associate each WMI class with its   
provider and with the MOF file for that provider.   
wmidiag.vbs CorrelateClassAndProvider   
Upon completion the utility will create a   
spreadsheet (WMIDIAG-\*\*\*\*.CSV) that will contain   
an enormous amount of data about each class,   
each provider, and each MOF file.  
Demo:  
\\\\gcrcssfs\\Public\\Commercial\\Platform\\ServerOS\\lewliu\\  
WMI

WMI remote connection  
• Use the wbemtest / wmimgmt.msc for remote connection test.  
• The Diagram – WMI / Windows firewall and DCOM   
Connecting Through Windows Firewall  
http://msdn.microsoft.com/en-us/library/aa389286(v=v  
s.85)

WMI remote connection setting  
• Connection 1  
1\. Ensure that the user account that is on Computer A is a local   
administrator on Computer B.  
2\. Allow for remote administration on Computer B.   
netsh firewall set service RemoteAdmin enable  
• Connection 2  
1\. If the Windows Firewall is enabled on Computer A, enable the   
"Allow Remote Administration" exception and open the DCOM   
port TCP 135 on Computer A  
netsh firewall add portopening protocol=tcp port=135   
name=DCOM\_TCP135  
2\. Add the client application or script, which contains the sink for   
callback to the Windows Firewall Exceptions List on Computer A.   
If the client is a script or a MMC snap-in, the sink is often   
Unsecapp.exe.

netsh firewall add allowedprogram program=%windir%\\system32\\  
netsh firewall add allowedprogram program=%windir%\\system32\\  
Part 3: WMI \- Advanced

Private Wbemcore.dll usage  
Method 1:  
replace the c:\\windows\\system32\\wbem\\wbemcore.dll in CD   
mode  
Method 2:  
1\. Stop and disable the wmi service  
2\. Rename the wbemcore.dll in  dllcache folder and c:\\windows\\system32\\  
wbem folder.   
3\. Copy the private dll to c:\\windows\\system32\\wbem  
4\. Open a cmd window and run   
rundll32 %WINDIR%\\system32\\wbem\\WMISvc.dll, MoveToAlone 4  
5\. Enable and start the wmi service  
6\. Change the wmi log to verbose mode  
7\. Run wbemtest to see if the private works.  
8\. If it works, enable the perfmon again, and monitor the box.

Example for Private Wbemcore.dll

WMISPY  
Used to record the WMI caller info (PID, TID), time, User account   
info.  
Download:  
file://tkfiltoolbox/tools/WMISpy/1.0.0.6/WMISpy\_1.0.0.6.zip  
1\. Copy WMISpy.exe to debugger folder  
2\. It needs download the symbols from Symbol server.  
3\. It also needs to separate WMI to a dedicated svchost.exe  
4\. Run ‘wmispy.exe c:\\wmilog.txt’  
5\. Repro the issue  
6\. Press “Ctrl \+ C” and “Enter” to stop logging.  
7\. Check the log for caller info.

WMISPY example

WMI Quota  
WbemTest \-\>Connect to Root \-\>Enum Instances:   
\_\_ProviderHostQuotaConfiguration  
• ThreadsPerHost: Defines the   
number of threads owned by any   
one host   
• HandlesPerHost: Defines the   
number of kernel object handles   
each host may have   
• ProcessLimitAllHosts: Defines the   
total number of host processes that   
can be executing concurrently   
• MemoryPerHost: Defines the   
amount of private memory that can   
be held by each host   
• MemoryAllHosts: Defines the   
combined amount of private   
memory (in bytes) that can be held   
by all hosts   
Memory and Handle Quotas in the WMI Provider Service  
http  
://blogs.technet.com/b/askperf/archive/2008/09/16/memory-and-h  
andle-quotas-in-the-wmi-provider-service.aspx

WMI Quota \- Job  
• The WMIPrvse.exe belongs to the same Job. All Wmiprvse.exe   
share the job Quota, which is defend in:   
\_\_ ProviderHostQuotaConfiguration  
We can check the Job info in procexp \-\> Wmiprvse.exe’s properties   
\> Job

TLS leak  
• Thread Local Storage (TLS) is the method by which each thread in   
a given multithreaded process can allocate locations in which to   
store thread-specific data.   
• Dynamically bound (run-time) thread-specific data is supported by   
way of the TLS API (TlsAlloc, TlsGetValue, TlsSetValue, and   
TlsFree).   
• TLS is finite resource (64 \+ 1024 Slots in total).

TLS leak debugging \-1  
WMIprvse.exe dump:  
• \!teb  
Tls Storage:            
LastStatusValue:        
0000000000000000  \< \-------- the Tls is NULL   
c0000017  \< \----- {Not Enough Quota}  Not enough   
virtual memory or paging file quota is available to complete the specified   
operation.  
• dt ntdll\!\_PEB 000007fffffdf000 TlsExpansionBitmap TlsBitmap  
\+0x078 TlsBitmap            
: 0x00000000\`77fa6920   \< \---- 1024 Slots   
\+0x238 TlsExpansionBitmap : 0x00000000\`77fa6910  \< \---- and 64   
expanded slot  
• dt ntdll\!\_RTL\_BITMAP 0x00000000\`77fa6920   
\+0x000 SizeOfBitMap     : 0x40  
\+0x008 Buffer             
: 0x000007ff\`fffdf080  \-\> 0xffffffff

• dt ntdll\!\_RTL\_BITMAP 0x00000000\`77fa6910   
\+0x000 SizeOfBitMap     : 0x400  
\+0x008 Buffer             
: 0x000007ff\`fffdf240  \-\> 0xffffffff  
TLS leak debugging \-2  
• dyb 0x000007ff\`fffdf240 0x000007ff\`fffdf240+(400/8)  
000007ff\`fffdf240  11111111 11111111 11111111 11111111  ff ff ff ff    
000007ff\`fffdf244  11111111 11111111 11111111 11111111  ff ff ff ff  
000007ff\`fffdf248  11111111 11111111 11111111 11111111  ff ff ff ff  
……  
000007ff\`fffdf2b8  11111111 11111111 11111111 11111111  ff ff ff ff  
000007ff\`fffdf2bc  11111111 11111111 11111111 11111111  ff ff ff ff  
• dyb  0x000007ff\`fffdf080  0x000007ff\`fffdf080+(40/8)  
000007ff\`fffdf080  11111111 11111111 11111111 11111111  ff ff ff ff    
000007ff\`fffdf084  11111111 11111111 11111111 11111111  ff ff ff ff

New Debugging options for WMI in   
Windows  
• There is a new global variable in Win for the Winmgmt.exe   
process which you can edit in the debugger to equal any   
error you are tracking in WMI.    
• At the point in time that WMI hits that error, the debugger   
will break on that function giving you the complete stack of   
functions which lead up to that error.  
• The new global variable is called   
g\_Error\_to\_Break\_on

New Feature: g\_Error\_to\_Break\_on  
1\. Separate the WMI to a dedicate Svchost.exe by:  
Winmgmt.exe  /standalonehost  
Net stop winmgmt & Net start Winmgmt  
2\. With proper symbols, break into the debugger (ctrl+break) and   
run command:   
ed wbemcomn\!g\_Error\_to\_Break\_on ERROR\_CODE   
NOTE: we need private symbol for wbemcomn.dll. Wbemcomn.dll is located   
in the c:\\windows\\system32 dir.  
3\. Type g (or F5) to release the debugger and allow WMI to   
continue processing until the issue occurs.  
4\. When the error occurs, the debugger will break into the WMI   
process and show the function call & ready for live-debug.

Example for g\_Error\_to\_Break\_on  
DEMO :  
• Error on querying class in Wbemtest:  
Select \* from Win32\_processor  
• Error message: 0x80041003  
T-Shooting:

1\. separate the Winmgmt to a new svchost.exe.  
2\. Config the private symbol for wbemcomn.dll  
3\. Attach to svchost.exe(get Pid by Sc queryex), and set break on error   
0x80041013.  
4\. Do query to repro the issue and get break in Windbg, save the dump   
by .dump /ma  
Demo – dump analysis for   
0x80041013  
• Dump share:    
\\\\gcrcssfs\\Public\\Commercial\\Platform\\ServerOS\\lewliu\\WMI  
1\. KERNELBASE\!DebugBreak+0x2  
2\. wbemcomn\!CMemoryLog::Write+0x81  
3\. WmiPrvSD\!CServerObject\_BindingFactory::InternalGetProvider+0x3c6  
4\. WmiPrvSD\!  
CServerObject\_BindingFactory::InternalGetProviderViaProxyRoute+0x295  
5\. WmiPrvSD\!CServerObject\_BindingFactory::InternalFindProvider+0x735  
6\. WmiPrvSD\!CServerObject\_BindingFactory::GetProvider+0x73a  
7\. wbemcore\!CWbemNamespace::DynAux\_ExecQueryExtendedAsync+0x24e  
8\. wbemcore\!CQueryEngine::ExecComplexQuery+0x4c0  
9\. wbemcore\!CQueryEngine::ExecQlQuery+0xcf  
10\. wbemcore\!CQueryEngine::ExecQuery+0x45c  
11\. wbemcore\!CAsyncReq\_ExecQueryAsync::Execute+0x68  
12\. …

Demo \- Continued  
• 0:011\> .frame /r 2; dv  
…  
WmiPrvSD\!CServerObject\_BindingFactory::InternalGetProvider+0x3c6:  
000007fe\`fa3dc68b 90                
nop  
this \= 0x00000000\`00000000  
…  
here.  
a\_User \= 0x00000000\`01503b40 "LEWIS-2K8-R2\\Administrator"  
a\_Locale \= 0x00000000\`020fc880 "ms\_409"  
a\_Scope \= 0x00000000\`00000000 ""  
a\_Name \= 0x00000000\`0196eef0 "CIMWin32“  \< \----- get the class name   
And actually, the problem is caused by a missing cimwin32.dll

Key point for g\_Error\_to\_Break\_on  
1: separate the WMI to a dedicated svchost.exe  
2: private symbol for Wbemcomn.dll which is used for setting break   
point  
\\\\gcrcssfs\\Public\\Commercial\\Platform\\ServerOS\\lewliu\\WMI\\internal pdb  
for wbemcomn.dll  
3: reoccurrence of the issue.  
4: save dump in windbg  
5: when exiting the windbg, the svchost.exe will exit too.  
6: seems only working for WMI error (0x8004\*\*\*\*).   
Demo: not working for DCOM error 0x80070005

# Questions

V1.How vhdx should be mounted ?

1\.	vhdx is mounted or not or not in use  
2\.	user should have read write access and share contributor role , SMB share elevated contributor   
3\.	copy the url of vhds directory from staorage account and conver it to path : \\\\safslogix.file.core.windows.net\\fslogixshare\\sid123\\profile\_user1.vhdx  
4\.	add the above to UNC PATH  
5\.	The vhdx will be mounted   
2\. how to diclipboard for rdp connection 

Local resources\>local devices and resources \> in rdp or msctc and uncheck the clipboard

fdisableclip \=1 , uncheck and and 0 clicked  (rdp tcp  available in two locations )

what is maximum number of collection can contained in 4 session hosts .

.shd and spl file difference ?

Wmi is a subset of Winrm . 

How can u find dll and mof files 

What are backward compatbilitby and forward computability ?  
What are rds cals and   
Wmi architecture and its service and startup type 

Dsregcmd /status \-\>to check machine is entra joined or hybrid joined

How to check port 3389 connectivity of a machine   
Tnc \-computername machine 1 \-port 3389

  frx version in C:\\Program Files\\FSLogix\\Apps.

Hierarchy of avd   
Tenant\>subscription \>resource group 

What are requirements of msix ?

Certificate msix packsgsing tool

How often should I turn my VMs on to prevent registration issues?  
After you register a VM to a host pool within the Azure Virtual Desktop service, the agent regularly refreshes the VM's token whenever the VM is active. The certificate for the registration token is valid for 90 days. Because of this 90-day limit, we recommend VMs to be online for 20 minutes every 90 days so that the machine can refresh its tokens and update the agent and side-by-side stack components. Turning on your VM within this time limit prevents its registration token from expiring or becoming invalid. If you started your VM after 90 days and are experiencing registration issues, follow the instructions in the Azure Virtual Desktop agent troubleshooting guide to remove the VM from the host pool, reinstall the agent, and reregister it to the pool.

Five common types of information available in Gateway Monitoring:  
   
\- Volume of incoming and outgoing traffic.  
\- Gateway uptime/downtime, Service health, errors, and failover events  
\- Active sessions and connection counts.  
\- Source and destination IP addresses and ports.  
\- Blocked or allowed requests, Firewall events.

6 troubleooting for cloud pc   
1\.	Check if the user has valid license  
2\.	Check if the user is added to the security group of the Provisioning policy   
3\.	Check the status of Provisioning Policy   
4\.	Check if the required URLs are whitelisted on the Client Machine.   
5\.	In case of ANC check if the ANC is Healthy   
6\.	ANC is configured correctly.   
Why to reprovision cloud pc ?

The Reprovision remote action lets admins reprovision Cloud PCs. This action can be useful when:  
•	You're testing different Cloud PC configurations.  
•	Your provisioned Cloud PC is misbehaving.  
•	The user simply wants to start from a fresh Cloud PC.  
The Reprovision action can also be used when a Cloud PC is in a Failed provisioning state in the Windows 365 provisioning node. You can think of reprovisioning as a similar process to resetting a physical device.  
When a Cloud PC is reprovisioned, the Cloud PC is deleted and recreated as a new Cloud PC. All user data, applications, customizations, and the like are deleted.  
The Cloud PC is reprovisioned to the current configured settings in the provisioning policy that's assigned to the user's Microsoft Entra group. If the image referenced by the policy changed, or if policy changed in any other way, the reprovisioned Cloud PC uses the new settings.  
For Windows 365 Flex Cloud PCs in Shared mode, you can bulk reprovision all the Cloud PCs in a provisioning policy. Admins can also select the percentage of Cloud PCs that are available for users to connect without being affected by the reprovisioning process.

# The Credential Security Service Provider (CredSSP)

The Credential Security Service Provider (CredSSP)

Is used for enabling SSO and NLA to prove that a user has the right to log on before the server creates a session. 

 

Network Level Authentication (NLA) is a new authentication method that completes user authentication before you establish a Remote Desktop connection and the logon screen appears. This is a more secure authentication method that can help protect the remote computer from malicious users and malicious software. The advantages of Network Level Authentication are:

It requires fewer remote computer resources initially. The remote computer uses a limited number of resources before authenticating the user, rather than starting a full Remote Desktop connection as in previous versions. 

By authenticating the user on the client machine before making the connection to the Terminal Server, NLA helps provide better security by reducing the risk of denial-of-service attacks. 

To use Network Level Authentication, all of the following requirements must be met: 

Remote Desktop Connection 6.0 or higher. 

The client computer operating system must support the new Credential Security Support Provider (CredSSP) protocol. Windows Vista and Windows Server 2008 support CredSSP by default, and Windows XP and Windows Server 2003 will add CredSSP support with a service pack. 

Windows Server 2008 Terminal Services. 

To use Network Level Authentication, you must meet the following requirements: 

The client computer must be using at least Remote Desktop Connection 6.0. 

The client computer must be using an operating system, such as Windows 7, Windows Vista, or Windows XP with Service Pack 3, that supports the Credential Security Support Provider (CredSSP) protocol. (Select here for enabling it on Win XP SP3). 

The RD Session Host server must be running Windows Server 2008 R2 or Windows Server 2008   

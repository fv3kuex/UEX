**Windows 365 / W365 / Cloud PC :-** 

\> **What is W365?** W365 is a cloud-based *Software-as-a-Service (SaaS)* that automatically creates a new type of Windows Virtual Machine (*Cloud PC*) for your end users. The Cloud PC lets users access Windows 10 or 11 from various devices (like Windows, iOS, and Android).   
W365 offers various solutions to support organizations of all sizes with multiple deployment options tailored to specific roles and needs.   
**Windows 365 is available in multiple editions:**   
**1.W365 Business \>** Simplifies deployment and is made specifically for use in smaller companies (up to 300 seats) who want ready-to-use Cloud PCs with simple management options.   
 **2\. W365 Enterprise \>** Is for larger companies who want unlimited seats for creating Cloud PCs. It includes options to **Create custom Cloud PCs** based on device images that you create, with more management options, and full integration with Microsoft Intune.   
**3\) W365 Government \>** Is a service that spans across a regulated US Government community cloud (GCC) and a public-facing cloud.   
**4\) W365 Frontline \>** Helps organizations save costs by providing a single license to provision up to three Cloud PCs for non-concurrent use. These are meant for cases where the user needs Cloud PC access for a limited time per day.   
**5\) W365 for Agents (Preview)**  **\>** Is the compute platform for AI-powered computer-using agents.   
   
**Feature | W365 | AVD** 

1. **Cost**   
* W365 \> Fixed monthly cost per user.   
* AVD \> Pay-as-you-go pricing based on usage.   
2. **Management**   
* W365 \> Can be managed through Intune but cannot be managed through Azure Portal.   
* AVD \> Can be managed through Azure Portal.   
3. **Managed by**   
* W365 \> Microsoft (Except for Azure Network when using ANC).   
* AVD \> Fully managed by customer (Except for AVD services).   
4. **Technical Skills required**   
* W365 \> Little to no VDI experience required.   
* AVD \> Requires detailed configuration knowledge.   
5. **User Self Service**   
* W365 \> Restart, Troubleshoot, Restore via Windows App and Web Client.   
* AVD \> Not supported. 

   
**\> What is a Cloud PC?** A Cloud PC is a highly available, optimized, and scalable virtual machine providing end users with a rich Windows desktop experience. It’s hosted in the W365 service and is accessible from anywhere, on any device.   
Users can connect to their Cloud PC by using windows.microsoft.com. Browser and app-based access is available for Windows, Mac, iOS, and Android devices.   
\> A Cloud PC provides customers a virtualized computing experience where you can stream your Windows experience from the Microsoft cloud to any device.   
\> Users have a **1:1 relationship** with their Cloud PC, making it their personal PC in the cloud.   
**Cost Management:-** 

* In **Azure**, you have to create a virtual machine and for this you have to pay charges. Also, you have to pay running charges.   
* But in **Cloud PC**, you don’t have to pay running charges. You need the appropriate license which you can purchase.   
* In Cloud PC, the VM is always running, hence no need to pay for running charges. 

**Key Benefits of Windows 365 for users:** 

1. **Platform Flexibility:** Allows access to personalized Cloud PCs from different platforms such as Windows laptop, iPad, Mac, Android.   
2. **High-speed internet access:** Cloud PCs are powered with rapid internet speed to ensure faster access to online services, reducing downtime and improving efficiency.   
3. **Self-service actions:** Allows the ability to perform self-service actions such as restarting, renaming, and troubleshooting their Cloud PCs, which enables users to resolve minor issues on their own.   
4. **Consistent device state:** It’s a feature that maintains a consistent device state (regardless of the number of days since last access), ensuring that users can continue their work without any loss of data or settings. 

   
**Key benefits of Windows 365 for administrators** 

* **No VPN needed \>** Hybrid devices are always connected to the on-premises network.   
* **Easy provisioning \>** Provision new devices in two steps.   
* **Regional policies \>** Use policies and connections based on the closest region.   
* **Consistent management \>** Manage Cloud PCs with Microsoft Endpoint Manager (MEM).   
* **Performance monitoring \>** Monitor performance using Endpoint Analytics.   
* **Scalable resources \>** Resize Cloud PCs based on available licenses.   
* **Enhanced security \>** Use MFA and conditional access.   
* **RBAC \>** The built-in RBAC role reduces security risks.   
* **Immediate security \>** Dedicated security baselines ensure protection for Cloud PCs. 

**Difference between W365 Business and Enterprise:** 

| Capability  | Windows 365 Business  | Windows 365 Enterprise  |
| ----- | ----- | ----- |
| **1\. Domain-Join**  | Microsoft Entra Join without Azure Virtual Network support  | Azure AD with the Azure AD Join with Azure support  |
| **2\. Policy Manager**  | Intune if user is licensed  | GPO and Intune MDM are supported  |
| **3\. Monitoring**  | Not Supported  | Endpoint Analytics, Reporting and Monitoring, Service Health and Operational Health Alert  |
| **4\. Troubleshooting**  | Not Supported  | Microsoft Intune includes Troubleshooting blade, Device Management Action  |

**W365 Enterprise Requirements:** 

1. **License \>**   
1. Intune Admin (To manage the devices).    
    Windows E3, Intune, Microsoft Entra ID P1, and Windows 365 (To use Cloud PC).   
2. **Azure Subscription \>** Active Azure Subscription (Not needed for Microsoft hosted network).   
3. **Network \>** i) Microsoft hosted network included. ii) Azure Virtual Network (hybrid requires line of sight to DC). 

**4\. User Identity \>** 

* Synced or cloud-only uses Microsoft hosted network   
* ii) Synced user \= hybrid 

   
**5\) Image \>** 

* Gallery or custom (Gen2) generalization / single 

**6\) Management \>** 

* Microsoft Endpoint Manager   
* (Supports SCCM Co-management) 

**W365 Editions:**   
\> **W365 Enterprise \>** No limits, for larger orgs \>    
**W365 Business \>** Max up to 300, for smaller business \>    
**W365 Frontline \>** 

* Shared \> Up to 3 sessions per license, shared (Non-concurrent) Device ID will be same for all users for one day   
* Dedicated \> Up to 25 users Can get dedicated session (Non-concurrent) 

\> **W365 Govt \>** For US Govt. & Public facing cloud \>    
**W365 For Agents (Preview) \>** For AI Agents   
**Pointers: – W365 Enterprise Uses:** 

1. Microsoft Intune to manage the Cloud PCs.   
2. Microsoft Entra ID for identity and access control.   
3. Azure Virtual Desktop for remote connectivity. 

**Pointers:** 

* Each Cloud PC is assigned to an individual user and is their dedicated Windows device.   
* When a W365 license is assigned to a user: i) Provisioning of a new Cloud PC automatically starts. ii) The Cloud PC is enrolled into Microsoft Intune. 

**\> What is Provisioning in W365?** Provisioning is the automatic creation of Cloud PCs for your end users.   
**Pointer:**    
After setting up Cloud PC support in Microsoft Intune, a Cloud PC is automatically provisioned whenever a user is assigned with a Cloud PC license to an appropriate MS Entra User group.   
To setup Cloud PC support we can use **MS hosted network (MHW)** or create **Azure network connections (ANC)**.   
\> **ANC** are links between Cloud PC and on-premise resources.   
\> We can choose a built-in Windows image (*gallery image*) or create our own custom image to use for each Cloud PC.   
\> You can run one Cloud PC with one license. Only one user can access Cloud PC, but it gives Admin a **10 minutes window** to take a session.   
\> Cloud PCs are billed in a **per-user per-month cost model**.   
\> Cloud PCs are either:   
 i) Joined to your enterprise AD domain & synced to Microsoft Entra ID. I   
i) Directly joined to Microsoft Entra ID.   
**W365 Business :-** W365 Business is a version of W365 made specifically for use in smaller companies (up to 300 seats). It offers an easy & streamlined way of providing Cloud PCs to your users.   
\> You don’t need to create a provisioning policy to create a Cloud PC using W365 Business. To provision a Cloud PC you can simply assign the W365 Business license to the user.   
\> You don’t need an Azure subscription or DC because everything works with Azure AD natively.   
\> Provisioning policy can take up to 2 hours for creating Cloud PCs. 

* A user will get **7 days grace period** to re-assign the license.   
* In case license is not re-assigned, the Cloud PC will automatically be deleted.   
* In grace period, user can access Cloud PC without any problem. 

**\> ANC (Azure Network Connection) :-** ANC are links between Cloud PCs and on-premise resources. ANC lets you provision Cloud PCs that are attached to a virtual network that you manage. 

* You can have up to **50 ANC per tenant**. 

As part of the connection process, the Windows 365 service is granted the following permissions:    
\> **Reader Permission** on the Azure Subscription.    
\> **W365 Network Interface Contributor** role on the specified resource group.    
\> **W365 Network User** role on the virtual network.   
**Requirements:**    
To create an ANC, you must meet these requirements:– 

1. Have the **Intune Administrator** or **Windows 365 Administrator** role.   
2. Have an **AD user account** with sufficient permissions to join the AD Domain into this Organizational Unit (hybrid Microsoft Entra Join ANC only).   
3. To create the first ANC, you must have the **Subscription Owner** or **User Administrator** role in the subscription where the Azure Virtual Network resides.   
   For any subsequent ANCs, only one **Subscription Reader** role is required. 

**4\)** For Disaster Recovery (DR) purposes, make sure that there are at least **50% of the IP addresses available** in your subnet.   
**5\)** Make sure that your **PowerShell execution policy** is configured to allow *RemoteSigned* scripts. 

* If you see Group Policy to set execution policy, make sure that your Group Policy Object (GPO) targeted at the Organizational Unit (OU) defined in the ANC is configured to allow *RemoteSigned* scripts. 

**\> W365 Frontline :-**    
W365 Frontline is a version of Windows 365 that helps organizations save costs by letting them provision a Cloud PC that can be used by multiple users with a single license.   
**W365 Frontline has 2 different modes:** 

1. Dedicated mode   
2. Shared mode 

**W365 Frontline in Dedicated mode:** 

* A single license in Dedicated mode: i) Lets you provision up to three Cloud PCs that can be used non-concurrently; each assigned to a single user. ii) Provides one concurrent session. 

**\> Use Case of Frontline Dedicated mode \>** W365 Frontline dedicated mode is designed specifically for workers who need a dedicated Cloud PC but don’t need 24/7 access.   
Frontline Cloud PCs in dedicated mode can be helpful for users who are: 

* On a rotation schedule.   
* Working across time zones and regions.   
* Part-time workers.   
* Contingent staff.   
  The maximum number of active **W365 Frontline Cloud PC sessions** in your organization is equal to the number of W365 licenses that you purchased.   
     
  For example, if you purchase **10 licenses**, up to **30 Cloud PCs** can be provisioned in dedicated mode, but only **10 of those Cloud PCs** can be active at a given time.   
     
  The active sessions are managed automatically. When a user signs off from their Cloud PC, the session is released for another user to start using their Cloud PC.   
     
     
  W365 Frontline in dedicated mode includes a **concurrency buffer** to let a tenant temporarily exceed the maximum concurrency limit for W365 Frontline Cloud PCs. 

1.The concurrency buffer can be used up to **four times per day** with a maximum of **one hour in each instance**.    
This hour starts from the moment the tenant exceeded the max concurrency limit.   
**emporary blocks :-** Excessive use of the concurrency buffer temporarily blocks its function for the next **48 hours**. A temporary block is implied when: 

* The concurrency buffer is used for more than one hour on **four or more occasions** within a 24-hour period. 

**Permanent blocks :-** If the tenant is temporarily blocked more than **two times in a seven-day period**, the tenant is permanently blocked from using the concurrency buffer.   
**NOTE :-** \> While temporarily or permanently blocked, you can still use your Frontline Cloud PC up to the maximum concurrency limit.    
\> Maximum concurrency limit is one.   
**Note :-** \> W365 Frontline Cloud PC is dedicated and automatically powers off after the user signs off from the Cloud PC, and is powered on when the user attempts to connect.   
\> After log off, machine will be powered on automatically for **2 hours** and after 2 hours it will be off.   
\> Restart will be done as per prediction and restart will be **before 30 minutes**.   
**W365 Frontline in shared mode :-** A single license: • Lets you provision one Cloud PC that can be shared non-concurrently among a group of users. • Provides one concurrent session.   
**W365 Frontline in shared mode is designed specifically for workers who:** • Require access to a Cloud PC to perform specialized tasks for a short time during their work day. • Don’t require data persistence.   
**Use Cases of Frontline Shared Mode :-**    
• Customer-facing workers. • External contractors.   
For example, if you assign **10 W365 Frontline shared licenses**, **10 Cloud PCs** can be provisioned for the group. 

* Only a single user can connect to a shared Cloud PC at a given time.   
* When a user signs out from the Cloud PC, all user data is deleted and the Cloud PC is released for another user to start using. 

**Note:** \> Concurrency buffer doesn’t exist for a Frontline Cloud PC in shared mode. \> With Frontline Cloud PCs in shared mode, you can also provide Cloud apps to users instead of the full Cloud PC desktop experience.   
**NOTE:** Frontline Cloud PCs in dedicated mode are prioritized over shared mode in the case where you already have provisioning policies created and later add licenses.   
\> When a tenant has both **dedicated mode** and **shared mode** provisioning policies and additional licenses are added, **dedicated mode Cloud PCs** are provisioned first.   
\> The **concurrency buffer** is available for dedicated mode only but is not available for shared mode. GPU-enabled Cloud PCs are also excluded from the concurrency buffer.   
**Features not yet supported in W365 Frontline:** • Resize a Cloud PC remote action. • Cross region disaster recovery.   
**NOTE:** You can confirm your license quantities under **Billing \> Your Products**.   
**License Requirements :-** To use W365 Frontline, each user must be licensed for: • Windows 11 Enterprise or Windows 10 Enterprise. • Microsoft Intune. • Microsoft Entra ID P1.   
These above licenses are available independently and also included in:– 

* **MS 365 E3**   
* **MS 365 E5**   
* **MS 365 F3**   
* **MS 365 A3**   
* **MS 365 A5**   
* **MS 365 Business Premium**   
* **MS 365 Education Student Use Benefit Subscription** 

\> **User Experience Sync** works for W365 Frontline in shared mode   
**User Experience Sync:** 

* It is a cloud-native solution that delivers a seamless and consistent experience for users across Cloud PC and Cloud App Session.   
* User Experience Sync is only available for W365 Frontline in shared mode.   
* User Experience Sync creates and attaches individual user storage to shared Cloud PCs. When a user signs in, their individual storage is attached, providing access to their settings, files, and application data. 

\> Can be enabled from provisioning policy by creating provisioning policy and selecting **Enable User Experience Sync** in the configuration section.   
**NOTE :-** •    
Enabling or disabling **User Experience Sync** requires the assignment to be removed and re-added. This will deprovision all Cloud PCs and provision new Cloud PCs.   
⇒ **Windows 365 Cloud Apps :-**    
Windows 365 Cloud Apps allow administrators to give users secure access to individual apps hosted on a Cloud PC, without requiring a dedicated Cloud PC for every user.   
• W365 Cloud Apps run on W365 Frontline Cloud PCs in shared mode, so **Windows 365 Frontline licenses** are required to use Cloud Apps.   
⇒ **Prerequisite :-** 

1. To create Cloud Apps you need a **W365 Frontline License**.   
2. To create Cloud Apps from custom applications that are not available on gallery image, you can use a **custom image**. 

**W365 Business :-** \> **Downsides :-** 

1. It is limited for up to **300 seats**.   
2. Business Cloud PC cannot be managed from **Intune**.   
3. You cannot configure Cloud PCs for **hybrid joined scenario**.   
4. No support for **custom image**. 

**Cloud PC Provisioning Status :-** 

1. Failed \> CPCs that failed to provision.   
2. In Grace Period.   
3. Provisioned.   
4. Provisioned with warning.   
5. Provisioning.   
6. Not Provisioning.   
7.    
   **Cloud PC Resize :-** 

\> **What does Resize mean?** Resize is a key product functionality that provides admin with anytime flexibility to upgrade a user’s Cloud PC’s spec. \> VCPU \> RAM \> Disk Size   
• Resize operations don’t require reprovisioning of the Cloud PC.   
 • Resizing doesn’t let you downsize disk space.   
\> **What is not supported?**    
Resizing doesn’t support **GPU Cloud PC**.   
• GPU Cloud PCs might show up in the resize flow but trying to resize a GPU Cloud PC results in an error.   
**NOTE :-**   
 \> Resizing disconnects user sessions, risking unsaved work and requires advance notification to prevent data loss.    
\> Resizing can take from **15 to 20 minutes** before the user can access their Cloud PC again.   
**Cloud PC Restore :-**   
Point-in-time restore lets an administrator restore a Cloud PC to the exact state it was at an earlier point in time. Restore points are automatically created at regular intervals based on how you have configured it.   
\> Point-in-time restore can be configured in a new or existing user setting. All users or groups assigned to the user setting have permission to use the point-in-time restore feature.   
**Restore Point Options :-** \> Short-term restore points \> Long-term restore points \> On-demand manual restore points   
**Short-term restore points :-** You can choose to set short-term restore points every 4, 6, 12, 16 or 24 hours. Each Cloud PC in the assigned groups has 10 short-term restore points saved at the intervals that you define in the user setting.   
For example: if you chose **4-hour intervals**, each assigned Cloud PC has **10 restore points** spread out every 4 hours over the last 40 hours.   
**Long-term restore points :-**   
There are also **four long-term restore points** that aren’t configurable. These long-term restore points are saved every **seven days**.   
**On-demand manual restore points :-**   
Manual restore points let administrators create a restore point whenever they want, for both single Cloud PC and groups of Cloud PCs.   
**Use case (On-demand manual restore point):** i) For creating a backup before taking management actions. ii) During employee off-boarding with sharing a restore point.   
**Important :-** • Only administrators can create a manual restore point. • Each Cloud PC can have only one manual restore point at a time. • A manual restore point has an expiration date of approximately **28 days**.   
**Grace Periods :-**   
When a CPC is in a grace period, the user can continue using the CPC for **7 days**. After the 7-day grace period expires, the user is logged off the CPC. They’ll lose access and the CPC will be deprovisioned.   
**Cloud PC Move :-** 

* We can move Cloud PCs in a policy format by editing the provisioning policy.   
* Cloud PCs can be moved: \> One region to another single region. \> One ANC to another ANC. \> A Microsoft hosted network to an ANC and vice versa.   
* Moving CPCs doesn’t result in reprovisioning.   
* Moving a CPC to a new region or ANC deletes the restore points in the older region or ANC and they are no longer accessible. 

**Cloud PC Reprovision :-**   
\> **What is Reprovision in Cloud PC?**    
The Reprovision remote action deletes a user’s current Cloud PC and creates a brand new Cloud PC for the same user.   
\> To Reprovision a Cloud PC, it must have a status of **Failed** or **Provisioned** in the W365 provisioning mode.   
   
**WebSocket connection flow:** 

1. User opens RD Client or web client and makes a feed request to RD Web.   
2. RD Web redirects the client Azure AD to receive a valid token for the service.   
3. If AAD is the authentication engine:   
* User is asked to enter their creds and that is passed to AAD.   
* If auth passes, then AAD issues a token to the RD Client.   
4. If AAD is not the final authentication engine (uses third party SSO/Identity Management Provider, for example Ping/Octa):   
* AAD responds with a redirect to SSO/Identity Management Provider .   
* RD Client communicates with Ping and user enters their creds.   
5. User opens a remote app or desktop on the client.   
6. The request goes to AFD (azure front door) which redirects the connection to nearest gateway based on source ip address   
* Connection will use the gateway assigned to the client   
* Note: Client may not always go to close gateway based on geo location. Its based upon latency from Front Door to all instances of the gateway and existing user load. So for a user geographically located in India its possible that Singapore has lower latency at times and hence user traffic will be sent via there. 

7.The gateway validates the user connection and contacts a broker   
8.The broker has a continuous connection to the RDAgent running on the Cloud PC.   
9.The broker provides the details needed to create the session to the RD Agent   
10.The RDAgent instructs the SxS Stack to reach out to the specific gateway   
11.RDP Stack then sends the session to the user via the gateway   
    
    
**UDP Connection flow:**   
All connections begin by establishing a TCP-based reverse connect transport over the Azure Virtual Desktop Gateway. Then, the client and session host establish the initial RDP transport, and start exchanging their capabilities. If RDP Shortpath is enabled on the session host, the session host then initiates a process called candidate gathering: 

1. The session host enumerates all network interfaces assigned to a session host, including virtual interfaces like VPN and Teredo.   
2. The Windows service Remote Desktop Services (TermService) allocates UDP sockets on each interface and stores the IP:Port pair in the candidate table as a local candidate.   
3. The Remote Desktop Services service uses each UDP socket allocated in the previous step to try reaching the Azure Virtual Desktop STUN Server on the public internet. Communication is done by sending a small UDP packet to port 3478\.   
4. If the packet reaches the STUN server, the STUN server responds with the public IP (specified by you or provided by Azure) and port. This information is stored in the candidate table as a reflexive candidate.   
5. After the session host gathers all the candidates, the session host uses the established reverse connect transport to pass the candidate list to the client.   
6. When the client receives the list of candidates from the session host, the client also performs candidate gathering on its side. Then the client sends its candidate list to the session host.   
7. After the session host and client exchange their candidate lists, both parties attempt to connect with each other using all the gathered candidates. This connection attempt is simultaneous on both sides. Many NAT gateways are configured to allow the incoming traffic to the socket as soon as the outbound data transfer initializes it. This behavior of NAT gateways is the reason the simultaneous connection is essential.   
8. After the initial packet exchange, the client and session host may establish one or many data flows. From these data flows, RDP chooses the fastest network path. The client then establishes a secure TLS connection with the session host and initiates RDP Shortpath transport.   
9. After RDP establishes the RDP Shortpath transport, all Dynamic Virtual Channels (DVCs), including remote graphics, input, and device redirection move to the new transport. 

    
   
   
   
   
   
   
   
   
   
   
   
 


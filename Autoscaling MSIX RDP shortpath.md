# Auto Scale

(Portal) Hostpool \> Settings \> RDP properties \> Advance \[Property value\] Hostpool \> Networking \> RDP shortpath \> \[RDP shortpath configure path\] Hostpool \> Auto scaling

Create a resource \> Search scaling plan \> Configure scaling plan.

**Autoscale:** Autoscale lets you scale your session host virtual machines in a host pool up or down to according to schedule to optimize deployment costs. (by turning on and off session hosts) Dynamic autoscaling is only available in Azure and isn't supported in Azure Government.

1. Scaling plan can be configured for Ramp-up and Ramp-down.  
2. Scaling plan can be assigned to multiple hostpools in the same resource group.  
3. A host pool can have only one scaling plan assigned to it.  
4. Scaling plan diagnostics can be enabled for troubleshooting purposes.  
5. A scaling plan can have multiple schedules.  
6. To assign a scaling plan "Desktop Virtualization Power On Off contributor" role needs to be assigned to service principle (AVD) on the subscription.

**AVD Definition:** Azure Virtual Desktop is a desktop and application virtualization service which runs on Azure.

**AVD Function:**

1. Full windows experience: \- AVD delivers a full \[Windows 11 or Windows...\]

---

**Start VM on Connect:** It lets you reduce cost by enabling users to turn on their session host (VMs) only when they need them.

**Setup "Start VM on Connect":** i) Start VM on Connect can be setup for both personal and pooled sessions. ii) A role is needed for this "Desktop Virtualization Power On Contributor". This role is assigned on the subscription.

1. This could be setup from Azure portal and Powershell/CLI. iii) RBAC role should be assigned to AVD service principle.

# MSIX App Attach

**MSIX App Attach.**

**Q. What is app attach** 

**Ans.** App attach enables you to dynamically attach applications from an application package to a user session in AVD.

1. Applications are not installed locally on session host or image, making it easier to manage custom images for the session host reducing operational overhead and cost for the organisations.  
2. Applications run within containers which separate user data in the operating system, which provides increasing security and making them easier to troubleshoot.

**Benefits of App Attach:** Applications are delivered using RemoteApp or as a part of a desktop session. Permissions are applied per application, per user, allowing greater control over which application user can access.

The same application package can be used across multiple host pool.

Applications can run on any session host running a Windows Client operating system in the same Azure region as the application package.

Applications can be upgraded to a new application version with a new disk image without the need of a maintenance window.

Users can run multiple versions of the same application concurrently on the same session host.

MSIX and Appx are Windows application package format.

MSIX app attach is microsoft's application layering technology, designed for AVD which uses the new MSIX package format to separate applications from the core OS and deliver applications to user dynamically.

**Features of Msix app attach**

1. Removes need of repackaging.  
2. It uses streaming technology to optimize bandwidth by only downloading the difference between an existing package and upgrade.  
3. Creates separation, the application lives inside a virtual application container which ensures strict isolation between the application and the OS.  
4. Supports silent installation: Capturing products is performed similarly to msi packaging. MSIX supports converting .exe installers and msipackages unattended.  
5. OS remains unmarked: Once the application is de-staged, no trace files are left within the system, like application was never there.  
6. Reduces time it takes for a user to login.  
7. Reduces infrastructure requirements and costs.

### **Requirements:**

1. AVD deployment  
2. Windows 11 single or multi-session  
3. Network Share (eg: Azure Files, Azure NetApp Files)  
4. Must have NTFS read permissions, SMB share read (RBAC)  
5. MSIX packaging tool  
6. MSIXmgr.exe tool  
7. Internal or external certificate with certificate name \= Organization name as required for the MSIX package.

It is recommended to create one file share per hostpool. It is not recommended to use file share within multiple hostpool. It is recommended to use NetApp files.

**MSIX AppAttach:** MSIX app attach is a way to deliver MSIX applications to Azure Virtual machines. MSIX app attach allows you to dynamically attach apps from an MSIX package to a user session.

### **Application State**

1. **Active:** MSIX package set to active makes application available to users.  
2. **Inactive:** MSIX set to inactive are ignored by AVD or are not added when user signs in.

### **Application Registration**

1. **On-demand:** Applications are partially registered at sign in and full registration is postponed until user starts the application. (Logon blocking)  
2. **Register at logon:** Each assigned application is fully registered during user log-on.

### **Disk Image Types**

For MSIX App attach images we can use CIMFS, VHDX and VHD.

VHD is not recommended because CimFS is faster than VHD and VHDX and consumes less CPU and memory.

It is recommended to use the Windows 11 session host while using CimFS.

**\[IMP\]:** All MSIX application packages include a certificate. The certificate needs to be trusted in the environment.

Self-signed (SS) certificates are supported with the appropriate chain of trust.

### **Role Assignment (RBAC)**

1. **RBAC roles with Azure files in Entra joined scenario:**  
   1. **Role:** Reader and Data Access  
   2. **To:** Azure Virtual Desktop, Azure Virtual Desktop ARM providers (AVD and AVD ARM Provider).  
2. **RBAC roles with ADDS:**  
   1. **Role:** Storage file data SMB share reader.

**MSIX is a packaging format | AppAttach is a delivery mechanism**

### **Terminologies**

1. **Expanding MSIX:** Taking an MSIX package (or output for conversion) and making it app attach package.  
2. **Staging:** Making an app attach package available on VM.  
3. **Registering:** Making a staged app attach package available for the user.  
4. **Delayed Registration:** Delay loading until user starts application.  
5. **Deregister:** Flush application data to profile.

### **Expanding MSIX package (Creating an MSIX App attach package)**

1. Obtain MSIX package (.MSIX file)  
2. Rename the .MSIX file to .zip  
3. Unzip the resulting file to a folder.  
4. Create a VHD with size equal to the size of the folder.  
5. Mount the VHD.  
6. Initialize the disk.  
7. Create partition.  
8. Format the partition.  
9. Copy the unzipped content into the VHD.  
10. Use MSIXMGR to apply ACLs on the content of the VHD.  
11. Unmount the VHD(X)/CIM.

### **Staging: It involves two steps.**

1. Mounting the VHD(x)/CIM to the VM.  
2. Notify the OS that the MSIX package is available for registration by mounting the apps to the Session host.

### **Registration: It is a per-user basis and explicit registration is required for that user.**

Registration is of two types:

1. **Regular registration:** Application assigned to a user is fully registered.  
2. **Delayed registration:** Each application assigned to the user is partially registered.

### **Deregister: It is a process of removing registered but non-running MSIX package for a user. This happens when user is logging off. During deregistration application data specific to the user is pushed to the local user profile.**

### **De-Stage:**

It notifies the OS that an MSIX package or application that currently isn't running and it is not staged for any user can be unmounted. This removes all reference to it in the OS.

### **MSIX Package: Structure**

**Package Payload:**

1. Application Files

**Footprint files:**

1. App Manifest  
2. App Blockmap  
3. App Signature  
4. Code Integrity

**App Payload:** The Payload files are the application code files and assets when building the image. eg: icon

**Appblockmap.xml:** The package block map file is a XML file containing a list of the applications files, including indexes and cryptographic hashes for each block of data stored in a package. The block file is verified and secured with a digital signature when the package is signed.

### **AppManifest.xml:**

The package manifest contains the information needed to deploy, display and update the MSIX application. The application information includes package identity, package dependencies, required capabilities, visual elements and extensible points (basic properties of file).

### **AppSignature.p7x:**

This file is generated when the package is signed. All packages are required to be signed before you run them (Validation).

# RDP Shortpath for AVD

### **RDP Shortpath for AVD**

RDP shortpath establishes a UDP based transport between a local device, windows app and session host in AVD. By default RDP begins a TCP-based reverse connect transport, then tries to establish a remote session using UDP.

If the UDP connection succeeds, the TCP connection drops. Otherwise TCP connection is used as a fallback connection mechanism.

UDP-based transport offers better connection reliability and more consistent latency.

TCP-based reverse connect transport provides best compatibility with various networking configuration and has a high success rate for establishing RDP connections.

**RDP Shortpath can be used in two ways:**

1. **Managed networks:** When direct connectivity is established between the client and session host when using a private connection, such as Azure ExpressRoute or a site-to-site VPN.  
   * A direct UDP connection between the client device and session host using a managed network is established in one of the following ways:  
     1. Need to enable RDP Shortpath listener and allow an inbound UDP port on each session host to accept connections.  
     2. Using the Simple Traversal Underneath NAT (STUN) protocol between a client and a session host. Inbound ports on the session host aren't required to be allowed.  
2. **Public Networks:** Where direct connectivity is established between the client and the session host when using a public connection. There are two types when using a public connection: a) A direct UDP connection using the Simple Traversal Underneath NAT (STUN) protocol between a client and a host. b) A relayed UDP connection using the Traversal Using Relay NAT (TURN) protocol between a client and a session host.

To use RDP Shortpath for managed network you must enable a UDP listener on the session host (port: 3390 by default) and it can be configured to use a different port.

**Q. Where we config the RDP Shortpath?** Hostpool \> Settings \> Networking \> RDP Shortpath


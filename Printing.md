WINDOWS PRINTING ARCHITECTURE AND PROCESS FLOW

**A Print Server is defined** as a printer attached to a local computer that is shared over a network to facilitate centralized printing services. The specific configuration of a computer sharing its local printer over a network characterizes that computer as a Print Server.

In the context of the Windows operating system, the term **Windows Printer** refers to a comprehensive system that enables software applications to produce hard copy output on a physical printing device. This process involves the conversion of digital data into a printable format, a task that is initially performed as an application generates a print job. The operating system is responsible for the systematic management of these print jobs, which includes the queuing of documents, the processing of data streams, and the subsequent transmission of data to a designated printer.

**The Windows printing architecture** is strategically designed to allow applications to print documents or jobs to a wide variety of printing devices. This architecture supports two primary classifications of printing: 

**Local Printing and Network Printing**. Local Printing involves the process of printing to a device that is directly connected to the client machine. A local printer is typically connected via physical or wireless interfaces such as USB, ETH, or WiFi.

The technical flow of a print job within the Windows environment is structured as follows:

![][image1]

When a user issues a print command from any application, the nature of the data determines the initial path. If the data consists strictly of plain text, it is directed to Winspool.drv, which is formally identified as the client spooler interface. However, if the data to be printed contains graphical elements, the request is first sent to the Graphical Device Interface (GDI). The GDI converts the complex graphical data into a standardized form that is compatible with Winspool.drv.

Upon completion of the conversion, the request is routed to Winspool.drv. Functioning as the primary client interface for the spooler, it invokes the Spooler Service, identified as Spoolsv.exe. The Spooler Service is a fundamental component of the Windows operating system that manages all active print jobs.

**Network Printing** involves the transmission of print data to a printer shared across a network infrastructure. In this scenario, the print job is dispatched from a client machine to a designated Print Server. This Print Server may be another client machine or a dedicated server device. This architecture enables multiple users to share a single printing resource effectively.

The printing process begins in earnest when the Spooler Service calls the Router, implemented as spoolss.dll. The Router performs the critical function of routing the print request to a specific and appropriate print provider. The Router determines which print provider to invoke based on the printer name or the handle provided in the job request. Once the determination is made, the Router calls the required print provider to manage the document flow.

Print Providers are implemented as dynamic link libraries (dlls). These components are generally categorized into two primary types:

1. localspl.dll: This component is responsible for managing print jobs for printers that are connected locally to the system.  
2. Win32spl.dll: This component is utilized to handle print jobs for printers situated on Windows network Servers.

Print providers ensure that print jobs are directed to their correct destinations, whether those destinations are local hardware devices or network-accessible printers.

Following the routing stage, the print job is directed to the Print Processor for final data transformation. The Print Processor stage is where the critical conversion from Enhanced Metafile (EMF) format to the Raw format occurs. The default print processor utilized within the Windows operating system is known as **WINPRINT**.

The Print Processor is responsible for converting the spooled data of a print job into a specific format that a print monitor can transmit to the physical printer. This conversion is essential because software applications generate print data in various formats, whereas physical printers require specific, standardized data formats to execute the print task.

In addition to data conversion, the Print Processor reads the spool file and performs necessary conversion operations on the data stream before writing the converted data back to the spooler. This component also handles application-level requests to manage the job state, such as pausing, resuming, or canceling active print jobs. After the conversion process is finalized, the Print Processor transmits the formatted data to the Print Monitor. The Print Monitor conducts checks on the available ports to assign the printer, ensuring the hardware receives the data correctly for physical output.

Print Monitor: The Print Monitor is the architectural component responsible for assigning print jobs to the specific port on which the printer is currently connected. The processed print job data is systematically sent from the Print Spooler to the printer. The system ensures that the data is transmitted directly, correctly, and reliably to the printer hardware. The Printer serves as the final destination and is the physical device that provides the hardcopy output to the user. \-\> In the context of Network Printing, the router calls the Win32spl.dll provider, from where the print job is directed to the spooler service (spoolsv.exe) of the specific machine which has the printer connected to it locally. From that point, the print job processes in the same manner as it does in the case of local printing. The print job is directed to the spooler service over port TCP-135 (RPC).

**Spooler Components.**

Application \-\> The Print application initiates the process and creates a print job by calling a GDI function. The Graphical Device Interface (GDI) is used by applications that require extensive graphic support or specialized data conversion. 

Winspool.drv \-\> This component functions as a client interface into the spooler subsystem. This is a driver file primarily responsible for calling the spooler service, which is identified as spoolsv.exe. 

Spoolsv.exe \-\> This component is implemented as a system service that is automatically initialized and started when the operating system is started. 

Spoolss.dll \-\> This library acts as a router, determining which specific print provider to call based on the printer name or handle supplied with each specific function call. 

Print Provider \-\> The print provider is the component responsible for directing print jobs to either a local or remote print device and managing the associated print queue. Print provider is of two types.

1. Local spl.dll \-\> This is a local print provider which handles all print jobs that are directed specifically to locally connected printers.  
2. Win32spl.dll \-\> All print jobs that are directed to a Remote Server are managed and handled by this provider. 

Print Processor \-\> These components are designed to convert spooled data from a print job into a data format that is correctly understood and processed by the print monitor. The default print processor provided with the operating system is WINPRINT. 

Print Monitors \-\> This component is responsible for directing the print data stream from the spooler to the appropriate port drivers for final transmission. It is also responsible for the systematic management and technical configuration of printer ports on a server.

**PRINT DRIVER** Print drivers are software components that translate Windows output data into a printer control language that a physical printer can interpret and understand. Print drivers are typically provided by printer manufacturers and are developed specifically based on the hardware model of the printer. 

Print drivers are categorized into two primary types**: Type-3 (Legacy) and Type-4 (modern).** 

**Type-3:** These are Vendor Provided Drivers that are also maintained and updated directly by the hardware vendor. 

**Type-4:** This driver type comes bundled with the Windows operating system and is updated automatically through the Windows Update service. 

**Print driver isolation:** Print driver isolation is a mechanism that allows administrators to troubleshoot performance or stability issues with the printer driver by isolating the printer driver within a separate container environment. The associated executable for this process is PRINTISOLATIONHOST.EXE.

\-\> System administrators can Enable branch office direct printing to optimize network traffic and printing efficiency in remote locations.

 \-\> It is important to define the RAW and EMF (Enhanced Metafile) datatypes and their roles within the printing subsystem. 

\-\> Understanding the technical process of conversion from RAW to EMF and vice-versa is critical for managing print data streams. 

There are three distinct kinds of **Driver isolation** available in the system configuration. 

(i) None: There is no isolation implemented in this specific scenario. 

(ii) Shared: In this configuration, multiple print drivers can be isolated together within a single shared container process. 

(iii) Isolated: In this configuration, a specific print driver is isolated individually in its own separate and dedicated container. 

**What is client side rendering**: (CSR) Client side rendering is a configuration setting which tells the system that print jobs should render on the client computer rather than the server. 

**Server Side Rendering:** In this operational mode, the print jobs are rendered and processed on the print server. 

**What is Point and Print.** \-\> 

Point and Print is a specialized method used for installing printers on client systems. In this method, the system will check if there is a compatible driver available in the local spooler repository. If a compatible driver is present, the system will install it directly from the local repository; if it is not present, the system will automatically download the necessary driver from the print server. 

**What is Print Nightmare.** 

Print Nightmare refers to the critical security vulnerabilities identified in the Windows Print Spooler service that could allow for remote code execution.

Print Nightmare is a known vulnerability in the Windows print spooler service which allowed attackers to execute code remotely and leverage privileges locally. To patch this critical vulnerability, Microsoft released a series of updates starting on July 6, 2021, which introduced the registry key RESTRICTDRIVERINSTALLATIONTOADMINISTRATOR, abbreviated as RDITA. This value was initially defaulted to '0' (disabled) until Microsoft released a subsequent update on August 10, 2021, where the value was defaulted to '1' (enabled). 

This registry setting, found at the path HKLM\\SOFTWARE\\Policies\\Microsoft\\WindowsNT\\Printers\\PointAndPrint, restricts standard users from installing printer drivers from print servers. 

Furthermore, the executable PRINTBRM.EXE is responsible for the migration of printers from one system to another. 

Initial communication between the print server and the client was conducted on port 445 (SMB), but this communication now occurs over port TCP-135 (RPC).

Section VI: Troubleshooting and Technical Specifications

The '11B' error is a common issue where both the client and server Windows installations need to be on the same patch or Windows update level; if they are not synchronized, users are likely to encounter this error when attempting to add printers. If the update levels are confirmed to be correct and the issue persists, it could indicate a network error or that the required port range is being blocked. 

Key acronyms in this domain include 

RDITA for Restrict Driver Installation To Administrator, 

XPS for XML Paper Specification, and 

RALPE for RPC Packet. Additionally, 

RPC Authn Level Privacy is typically Enabled (1) for secure communication. 

The Enhanced Metafile, or EMF, is an image description format defined by Microsoft that describes the appearance of text, graphical shapes, and sampled images on printed or displayed pages. RD Easy Print, which was previously known as TS Easy Print, is utilized for redirecting printers on a remote computer. For the redirected printer on the remote system, the driver is used to send the print jobs back to the locally connected printer.

A printer driver includes two primary functional components: the rendering component and the configuration component. 

The Rendering Component converts graphics commands from the software application into a specific format that the printer hardware understands. 

The Configuration Component contains the user interface that allows users to control printer-selectable options and includes the program interface that communicates the printer's specific configuration and features to an application. 

Enhanced Point & Print refers to the updated printer sharing mechanism that allows a print client to print to V4 shares without the necessity of downloading the manufacturer-provided device driver from the Print Server.

The Windows printing subsystem supports two generations of drivers with distinct characteristics:

**Type-3 / V-3 Drivers** are considered Legacy components. They are provided directly by printer manufacturers and vendors. 

Printer management for Type-3 is notably complex, requiring separate 32-bit and 64-bit drivers as well as different drivers for different hardware models. 

These drivers take more disk space due to the inclusion of custom configuration UI and language resources. 

Because they are developed independently by manufacturers, V3 drivers are historically more prone to system crashes. Updates for these drivers are provided solely by the manufacturer.

**Type-4 Driver**s represent the Modern architecture, often referred to as Print Class Drivers. These are bundled with the operating system and updated through Windows Update. 

Microsoft works closely with IHVS (Independent Hardware Vendors) for the development of V4 drivers. Printer management is significantly simplified as a wide range of printers are supported through common PDLs (Page Description Languages) such as PCL, PS, and XPS. 

They utilize smaller, Windows-provided components and are less prone to crashes due to their reduced size and modular nature. Updates are pushed seamlessly through Windows Update.

There are various methods available to add a printer from a Print Server using Point & Print:

1. Adding from the Printers and Scanners section within the Windows Settings menu.  
2. Adding via a UNC path.  
3. Utilizing the Drag and Drop method.  
4. Through a right-click selection.

POINT AND PRINT AND SYSTEM COMPONENTS

What is the Point & Print feature? It is a comprehensive feature that allows users to print to a remote printer on a network without having to manually download and install the printer driver on the machine. 

The Point & Print mechanism checks for an existing driver (printer driver) on a local driver store, and if it does not find a compatible driver, it downloads and installs the driver from the print server itself.

* Client downloads the configuration data from the server using GetPrinterDataEx calls when using V4 drivers.  
* The configuration data includes essential data files such as Generic Printer Description (GPD) files, PostScript Printer Description (PPD) files, and resource DLLs (RC.dll), etc.  
* The print systems examine the client and validate that the resource DLL (RC.dll) contains no executable code to ensure system security.  
* Splwow64.exe: This is a system process that allows 32-bit applications running on 64-bit Windows environments to interact successfully with 64-bit printer drivers.

PRINT PIPELINE AND NETWORK PROTOCOLS

***LPT \-\> Line Print Terminal (Parallel Port)*** 

**Print Filter Pipeline**: The Print Filter Pipeline is a component in XPSdrv print drivers that processes the XPS spool file and includes the processing filters and the filter configuration files. It represents the logical collection of printer driver filters that are specified in the filter configuration file. 

Terminal Services Printing (TS Easy Print) and Printer Redirection. 

Filter Pipeline:

 i) Specifies filters to load 

ii) Processes jobs 

iii) Unloads filters 

iv) Exits on process ends or another event. 

Filter Pipeline Configuration Services: PrintFilterPipelineSvc.exe 

WSD: Web Services on Devices is a specialized protocol for communicating with a device (printer) over the network. WSD allows network-connected IP-based devices to advertise their functionality and offer these services to clients by using the Web Services Protocol. WSD provides a network PnP (Plug and Play) experience that is similar to installing a USB device. 

Hardware ID: It is a unique identifier assigned by the enumerator associated with the port that the printer is connected to. A Hardware ID is a vendor-defined identification string that Windows utilizes to match a device to a specific inf file. A device can have more than one hardware ID associated with it.

DRIVERLESS PRINTING AND MOPRIA

Driveless Printer \-\> MOPRIA model MOPRIA Certified Printers: When a dedicated driver is not available, Windows can install MOPRIA certified printers without needing to install any additional software or drivers. What is MOPRIA? \-\> MOPRIA refers to the Mobile Printer Alliance.

Mopria is an alliance of printer manufacturers that are working together to define an Open Standard for Driverless Printing that can be used to make printing from mobile devices significantly easier. It is an implementation of the IPP (Internet Printing Protocol) Standard.

REGISTRY KEYS AND RENDERING MODES

Important Reg.

* HKCU \\ Software \\ MS \\ WINNT \\ CurrentVersion \\ Devices \-\> This key contains the list of all printers installed on the system.  
* HKLM \\ SYSTEM \\ CurrentControlSet \\ Control \\ Print \\ Environments \\ Printers

Q. **What is Client Side Rendering?** \-\> It is a feature that allows print jobs to be fully rendered on the client machine when targeting a shared printer hosted on a Windows client or server computer acting as a print server. After being rendered on the client, the print job is sent to the print server for queuing and printing by the print spooler without further server-side rendering required. 

Q. **When printing through the print server** instead of the job being rendered on the client, the job is sent to the server where the job gets rendered and subsequently sent to the print spooler for queuing and printing.

* Type-3 driver location in file The Type-3 printer driver location within the file system architecture is found at the specific path C:\\Win\\Sys32\\Spool\\drivers\\x64\\3.

Printer Redirection.

Printer Redirection is a specialized feature that enables a locally connected printer to be mapped onto a remote machine, allowing for printing capabilities across a network or over the internet. This functionality specifically enables the user to output documents to their locally installed printer while connected to a Terminal Server Session, which is formally identified as a Remote Desktop Session. The RDS client provides the underlying framework required to facilitate this redirection of data between the local hardware and the remote session environment.

\> Application : The Print application creates a print job by calling GDI Function

\-\> Graphics Device Interface: is used by application which require graphics support.

\-\> WinSpool.drv : This is the client interface into the spooler. it is a file which is related to the printer driver functionality within Windows operating system. the .drv extension. Signifies that it is a driver file and responsible for calling the spooler Service.

\-\> Spoolsv.exe \-\> This is the Spoolers API service and This is the Spoolers API server and it is implemented as a Service when operating System is started.

\-\> Spoolss.dll : It acts as a router determining which print provider to call based on a printer name or handle supplied with each function call. Then it passes the function call to the appropriate print provider.

\-\> Print Providers \-\> The Print Provider is responsible for directing print jobs to local or remote print devices and print queue management operations such as starting, stopping and enumerating print queue.

* Print Server \-\> A Printer attached to a local Computer that is Shared over network.  
  \-\> The Configuration of a Computer is share their local printer over network make this computer called as Print Server.  
* What is printing: Windows Printer refers to a system in windows operating system that enables application to produce hard copy output on a physical printing device.  
  This involves converting digital data into a printable format which is done through an application (generating print job)  
  The operating system is responsible to manage these print jobs by queuing processing and sending data to a printer.  
  \-\> The windows printing architecture is designed to allow applications to print documents (job) to various printing devices.  
  \-\> Windows Printing is of two types.  
* Local Printing. \-\> This involves printing to a printer directly connected to the client machine. A Local printer can be connected through USB/ETH/WiFi.

---

\-\> Network Printing: This involves printing to a printer shared over a network. The print job is sent from client machine to a Print Server. (Which could be a mother client machine or a dedicated print server device)

Network printing allows multiple users to share a single printer.

(Continued)

Here the printing process actually starts.

\-\> Spooler Service calls spoolss.dll which does the job of routing to a particular (appropriate) print provider.

\-\> Router determines which print provider to call based on the printer name or handle. Router then calls the required print provider.

Print Providers are implemented as dynamic link libraries (dlls). These are of two types.

Localspl.dll and Win32spl.dll.

Localspl.dll \-\> It handles print jobs for printers connected locally.

Win 32spl.dll \-\> It handles print jobs for printers on windows network Servers.

---

Windows Print

GDI \-\>

\[Application\] \--Plain Text--\> \[Winspool.drv\]

\[Application\] \--Graphic A--\> \[GDI\]

\[GDI\] \--\> \[Winspool.drv\]

\[Winspool.drv\] \--RPC--\> \[Spoolsv.exe\]

\[Spoolsv.exe\] \--\> \[Spoolss.dll\]

\[Spoolss.dll\] \--Print Providers--\> \[Local spl.dll\]

\[Local spl.dll\] \--\> \[Print Processor\] \--Winprint.dll EMF to RAW--\>

\[Print Processor\] \--\> \[Print Monitor\] \--Checks for ports to Assign Printer--\>

\[Print Monitor\] \--\> \[Printer\]

Printer flow

When we give a command to print on any application. if only the data is only plain text \- it goes to Winspool.drv which is called client spooler interface.

If the data to be printed contains graphics then the request goes to GDI. It converts the data to a form which is understood by Winspool.drv.

After conversion request goes to winspool.drv. It is a client interface for the spooler. It calls the Spooler Service (Spoolsv.exe)

Spooler Service is a crucial component of windows os that manages all print jobs.

\-\> Print providers direct print jobs to their correct destinations, weather those destinations are local printer or printers on a network.

\-\> The print job is then directed to the Print processor.

Print processor is where conversion from Enhanced metafile (EMF) to Raw format takes place.

NOTE: the default print processor in Windows is WINPRINT.

Print processor converts the spooled data of a print job into a format that a print monitor can send to Printer. This conversion is necessary because applications generate print data in various formats and printers require specific data formats to print.

Print Processor also reads the spool file performs necessary conversion operation on data stream and then writes converted data back to spooler. It also handles application request to pause, resume, cancel print jobs.

After conversion the print processor sends the data to print monitor.

Print Monitor: It is responsible for assigning the Jobs to port on which the printer is connected.

The processed print job data is sent from Print Spooler to printer.

It ensures that the data is transmitted directly correctly and reliably to the Printer.

Printer is the final destination and physical device that provides hardcopy output.

\-\> In Network Printing router calls the Win32spl dll (provider) from where the print job is directed to the spooler service (Spoolsv.exe) of the machine which has the printer connected to it locally.

From there the print job processes as it does in case of local printing.

The print job is directed to the spooler service over port TCP-135 (RPC)

Spooler Components.

i) Application \-\> The Print application creates a Print job by calling a GDI function. GDI is used by applications that require graphic support or conversion.

---

Winspool.drv \-\> This is a client interface into spooler. this is a driver file responsible for calling Spooler Service which is spoolsv.exe.

Spoolsv.exe \-\> It is implemented as a service when the Operating System is started.

Spoolss.dll \-\> It acts as a router determining which print provider to call based on the printer name / handle supplied with each function call.

Print Provider \-\> The Print provider is responsible for directing print jobs to local or Remote print device and print queue.

Print provider is of two types.

1. Local spl.dll \-\> This is a local print provider which handles all print jobs directed to local printers.  
2. WIN32 SPL.dll \-\> All print jobs directed to Remote Server are handled by this provider.  
   Print Processor \-\> These convert spooled data from a print job into a format that is understood by print monitor.  
   Default print processor provided with the

---

Processor is WINPRINT.

Print Monitors \-\> It is responsible for directing a print data stream from spooler to appropriate port drivers.

It is also responsible for management and configuration of Printer ports on a server.

* PRINT DRIVER  
  Print drivers is a software that translates windows output into a language that a printer can understand.  
  Print drivers are provided by the printer manufacturers based on the model of the printer.  
  Print drivers are of two types: Type-3 (Legacy) Type-4 (mod)  
  Type-3: Vendor Provided Drivers. (also updated by vendor)  
  Type-4: Comes bundled with operating system and it is updated through Windows Update.  
  Print driver Isolation: Print driver isolation is what allows administrators to troubleshoot issues with the printer driver by isolating the printer driver in a seperate container.  
  PRINTISOLATION HOST.EXE

---

\-\> Enable branch office direct printing.

\-\> What is RAW & EMF datatype

\-\> What is the process of conversion from RAW to EMF and vice-versa

There are three kinds of Driver isolation

(i) None: No Isolation in this scenario.

(ii) Shared: Multiple print drivers can be isolated in a shared container.

(iii) Isolated: Print driver is isolated in a seperate container.

What is client side rendering: (CSR)

Client side rendering is a setting which tells print jobs should render on client computer.

Server Side Rendering: In this print job are rendered on the print server.

What is Point and Print.

\-\> It is a method of installing printers on the client systems, but it will check if there is a compatible driver on the local spooler repository.

If present it will install it from the local repository if not it will download the driver from the print server.

What is Print Nightmare.

---

Print Nightmare is a known vulnerability in the print spooler service which allowed the attackers to execute code remotely and leverage privileges locally.

To patch this vuln. microsoft released update on july 6 2021 introducing the registry key RESTRICT DRIVER INSTALLATION TO ADMINISTRATOR (RDITA) This value was default to '0' (disabled) until microsoft released update on august 10, 2021 where this value was defaulted to '1' (enabled)

This setting / registry key (RDITA) Restricted standard users from installing the Printer drivers from Print Servers.

RDITA Path: HKLM \\SOFTWARE\\Policies\\Microsoft\\Windows NT \\Printers \\Point And Print.

\-\> PRINTBRM.EXE: It's responsible to migrate printers from one system to another.

\-\> Initial communication between print server and client was done on port 445 (SMB) Now this communication happens over port TCP-135 (RPC)

---

'11 B'

A Both client and server windows needs to be on the same patch / windows update If not, when trying to add printers they are likely to get an error 11 B

If everything is OK and still getting 11 B it could be a network error or the port range could be blocked.

RDITA \-\> Restrict Driver Installation To Administrator

XPS \-\> XML Paper Specification.

RALPE \-\> RPC Packet

RPC Authn Level Privacy Enabled (1)

EMF \-\> Enhanced MetaFile

\-\> It is an image description format defined by microsoft. It describes the appearance of text, graphical shapes and sampled images on printed or displayed pages.

RD Easy Print: Also previously known as TS Easy print It is used for redirecting printers on a remote Computer.

For the redirected printer (on the remote computer) the driver is used to send the print jobs to the connected printer.

---

Printer driver includes rendering component and configuration component.

Rendering Component \-\> It converts the graphics commands from the application into a format that the printer understands.

Configuration Component: It contains the user interface that allows users to control printers selectable options. It also includes a program interface that communicates the printers configuration and features to an application.

Enhanced Point & Print:

The updated printer sharing mechanism is referred to as enhanced Point and Print and it allows print client to print V4 shares without downloading the manufacturer provided device driver from the print server.

Type-3 / V-3:

1. It is Legacy  
2. It is provided by vendors. Or Printer manufacturers.  
   Type 4:  
   Modern. (Print Class Driver)  
   It is bundled with Windows update.

---

Type 3

3\) Printer management is complex e.g. 32 bit & 64 bit drivers, different drivers for differ models.

4\) Takes more space due to configuration. UI and language resources.

5\) Type 3 drivers are developed by Printer manufacturers.

6\) V3 drivers are more prone to crashes

7\) Updates for driver are provided by manufacturer.

Type 4

Printer management is simplified as wide range of printers are supported due to common PDLs e.g. PCL, PS, XPS

Smaller size windows provided components.

Microsoft works with IHVs for developing V4 drivers.

V4 is less prone to crashes due to reduced sizes.

Updates are pushed through Windows update.

What are the various methods of Point & Print.

* How can a printer be added from a Print Server.  
1. From Printers and Scanners (Settings)  
2. From UNC path  
3. Drag and drop. Right click.

---

Q. What is Point & Print feature.

Ans: It is a feature that allows users to print to a remote Printer on a network without having to manually download and install the printer driver on the machine. Point & Print checks for an existing driver (printer driver) on a local driver store and if it does not find a compatible driver it downloads & installs the driver from the print server itself.

* Client downloads the configuration data from the server using GetPrinterDataEx calls when using V4 drivers.  
* The configuration data includes data files like generic Printer description (GPD) files, Post script Printer description (PPD) and resource DLL (RC.dll) etc.  
* The Print Systems examines the client & validates the resource dll (RC.dll) contains no executable code.  
* SplWOW64.exe: It allows 32-bit apps running on 64-bit windows to interact with 64-bit printer drivers.

---

* LPT \-\> Line Print Terminal (Parallel port)  
  Print Filter Pipeline: The Print filter pipeline in XPSdrv print driver processes the XPS spool file and includes the processing filters and the filter configuration files. It is the Logical collection of printer driver filters that are specified in the filter configuration file.  
  V3 & V4 Drivers  
  Terminal Services Printing (TS Easy Print) Printer Redirection.  
  Filter Pipeline:  
  i) Specifies filters to load  
  ii) Processes jobs (iii) Unloads filters.  
  iv) Exits on process ends or another.  
  Filter Pipeline Configuration Services.  
  \-\> PrintFilterPipelineSvc.exe  
  Hardware ID:  
  WSD: Web Services on Devices is a protocol for communicating with Device (printer) over the network  
  WSD allows network-connected IP-based devices to advertise their functionality and offer these services to clients by using the Web Services Protocol.

---

WSD provides a network PnP experience that is similar to installing a USB device.

Hardware ID: It is an identifier assigned by Enumerator associated with the port that the printer is connected to.

A hardware ID is a vendor defined identification string that windows uses to match a device to an inf file.

A device can have more than one hardware ID associated with it.

Driveless Printer \-\> MOPRIA model

MOPRIA Certified Printers.

When a driver is not available windows can install MOPRIA certified printers without needing to install any additional software or drivers.

What is MOPRIA \-\> Mobile Printer Alliance.

* Mopria is an alliance of printer manufacturers that are working to define an Open Standard for Driver Printing that can be used to make printing from mobile devices easier.  
  It is an implementation of IPP (INTERNET Printing Protocol) Standard.

---

Important Reg.

* HKCU \\ Software \\ MS \\ WINNT \\ Current Version \\ Devices.  
  \-\> List of all printers installed.  
* HKLM \\ SYSTEM \\ CurrentControlSet \\ Control \\ Print \\ Environments Printers  
  Q What is Client Side Rendering \-\> It is a feature that allows print jobs to be fully rendered on the client when targeting a shared printer hosted on a windows Client / Server computer acting as a print server After being rendered on the client the print job is sent to the print server for querying and printing by the Print Spooler without further server-side rendering.  
  Q. When Printing through the Print Server instead of job being rendered on the client the job is sent to the Server where the job gets rendered and sent to the print spooler for queuing and printing.

---

* Type-3 driver location in file  
  C:\\Win\\Sys32\\Spool\\drivers \\x64 \\3  
  Printer Redirection.  
  It is a feature that allows a local printer to be mapped on a remote machine and allows printing across the network or internet.  
  Printer redirection enables the user to print to their locally installed printer from a Terminal Server Session (Remote desktop Session)  
  The RDS client provides.

---

Print and Document Services.

Print Services role

What's New in Print Management in Win 7\.

Print Scan & Fax Server Installation Guide

Print Management \- Step by Step

Assigning Delegated Print Administrator & Printer

Windows Printing Legacy

work with Enhanced Point & Print.

[image1]: <data:image/png;base64,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>
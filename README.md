# Windows Server 2025 Home Lab

This is my personal project where I built a mini version of a company computer network, all inside my own PC, using free software. I set up a server, connected another computer to it, and configured a bunch of things that real IT professionals use every day at their jobs.

**Author:** Clinton Kehinde
The complete step-by-step build, including screenshots and detailed explanations, is available here:

---

## What is this project?

Big companies use something called a "server"  basically a powerful computer that manages everyone else's computers, logins, passwords, shared files, and internet settings. I wanted to learn how that works, so I built my own server at home using a program called VMware, which lets you run a "fake" computer (called a virtual machine) inside your real computer, without messing anything up.

Think of it like a game inside a game I have my real laptop (Windows 11), and inside it, I created a pretend server computer (Windows Server 2025) that I could experiment on safely. If I broke something, I could just undo it or start over — no real damage done.

## My Setup

| What | Details |
|---|---|
| My computer | Windows 11 laptop |
| Processor | Intel Core i5 (4 cores) |
| Memory | 8 GB RAM |
| Storage | 256 GB SSD |
| Virtual machine software | VMware Workstation (free) |
| Server software | Windows Server 2025 (free 180-day trial) |

## What I Actually Built

- Set up a virtual server computer inside VMware
- Installed Windows Server 2025 on it
- Made the server keep the same address on the network at all times (a "static IP") so other computers could always find it
- Turned my server into a **Domain Controller** — basically the "boss" computer that manages logins for every other computer on the network
- Set up **DNS**, which is like a phonebook that turns computer names into numbers (IP addresses) so devices can find each other
- Set up **DHCP**, which automatically hands out addresses to any device that joins the network, kind of like a receptionist giving out visitor badges
- Created folders to organize fake "employees" — usernames, groups, and permissions, just like a real company would
- Added a second server and connected it to the first one, so they could work together
- Used **Group Policy** to apply one rule to many computers at once (for example, blocking access to Control Panel for regular users)
- Combined 5 small virtual hard drives into one big protected drive, so that if one drive fails, no data is lost (this is called Storage Spaces / RAID-style storage)
- Set up a **file server** a shared folder that other computers on the network could open, use, and save files to
- Controlled exactly who could open, edit, or delete files using **permissions**
- Set up a **firewall** to control what traffic is allowed in and out of the server (like a security guard checking IDs)
- Turned on and tested **Microsoft Defender Antivirus** to protect against viruses and malware
- Set up automatic **backups** so that if something breaks, I can recover the lost data
- Learned how to read the server's **activity logs** (Event Viewer) to figure out what went wrong when something doesn't work

## How the Documentation is Organized

I wrote everything down in the order I actually did it, step by step, explaining not just *what* I clicked, but *why* I did it. It's broken into 17 sections:

1. Setting up the virtual machine
2. Installing Windows Server
3. Basic first-time setup
4. Understanding "roles" and "features" (what a server can be used for)
5. Setting up Active Directory (the login/user management system)
6. Creating DNS records (the "phonebook" entries)
7. Creating user accounts, groups, and folders to organize them
8. Connecting a second server to the network
9. Group Policy (applying rules to many computers at once)
10. Setting up DHCP (automatic address assignment)
11. Building protected storage out of multiple hard drives
12. Setting up a file server people can share files on
13. Controlling who can access which files
14. Managing Windows updates
15. Reading system logs to troubleshoot problems
16. Setting up the firewall
17. Setting up antivirus protection

The full write-up with all the details and steps is in the project document in this repo.

## Skills I Practiced

- Setting up and managing virtual machines
- Managing user accounts and permissions like a real IT admin
- Understanding how computers find each other on a network (DNS/DHCP)
- Automating settings across many computers at once (Group Policy)
- Protecting data using permissions, backups, and antivirus
- Reading logs to solve problems, instead of just guessing
- Setting up secure file sharing between computers

## A Few Notes

- This was built purely for learning — it's not a real company network, so a few settings (like the security software being turned off temporarily) were only okay because it's just my personal lab, not something I'd do at a real job.
- I kept things simple since it's a small home lab, but I followed the same basic structure and naming style that real companies use.

## What I Might Add Next

- A second "boss" server for backup, in case the first one goes down
- A system for automatically installing updates on lots of computers at once
- Automatically connecting shared folders when someone logs in
- More organized folders for a bigger, more realistic setup
- A certificate system for extra security
- A second virtual machine host to simulate multiple office locations

- Section 1: Setting Up the Virtual Machine in VMware
Before I could install Windows Server 2025, I first needed to create a virtual environment where it could run. Rather than installing Windows Server directly on my computer, I chose to use VMware Workstation, which allowed me to create a virtual machine (VM). A virtual machine behaves like a completely separate computer, with its own processor, memory, storage, and network connection, while sharing the hardware of my Windows 11 PC.
One of the biggest advantages of using a virtual machine is that I could experiment freely without affecting my main operating system. If I made a mistake, I could simply restore a snapshot or rebuild the server, making it the perfect environment for learning and testing.
Step 1. Download VMware Workstation
The first task was to install the virtualisation software.
1.	I visited the VMware website and created a free account.
2.	I downloaded VMware Workstation.
3.	After the download completed, I ran the installer, followed the installation wizard, and restarted my computer when prompted.
Once VMware was installed, I was ready to create my virtual server.
________________________________________
Step 2. Download the Windows Server 2025 ISO
With VMware installed, I needed the Windows Server installation media.
4.	I visited Microsoft's Evaluation Center and searched for Windows Server 2025.
5.	After completing the registration form, I selected the ISO download option.
6.	Once the download finished, I saved the ISO file in a dedicated folder on my D: drive so it would be easy to locate later.
Having the ISO downloaded meant I had everything required to build my virtual server.
________________________________________
Step 3 – Create the Virtual Machine
I then launched VMware Workstation and created my first virtual machine.
7.	I selected Create a New Virtual Machine and chose the Typical configuration before clicking Next.
8.	I selected I will install the operating system later, then clicked Next.
9.	For the operating system, I selected Microsoft Windows, followed by Windows Server 2025 from the version list.
10.	I named the virtual machine server01 and selected a location on my computer where the virtual machine files would be stored. Before continuing, I confirmed there was enough free storage available, as Windows Server and future lab files would require a reasonable amount of disk space.
11.	I configured the virtual hard disk with a capacity of 60 GB and chose Store virtual disk as a single file. Although Windows Server can run with less storage, allocating additional space provides flexibility for installing server roles, creating shared folders, and expanding the lab later.
12.	Before completing the wizard, I selected Customize Hardware so I could adjust the virtual machine's hardware settings.

Configuring the Virtual Hardware
To give the virtual machine enough resources for the services I planned to install, I configured the hardware as follows.
Memory (RAM)
I allocated 8 GB (8192 MB) of memory.
Although Windows Server can operate with much less memory, assigning 8 GB provided a smoother experience while running services such as Active Directory, DNS, DHCP, and File Services simultaneously.
Processors
I assigned 4 processor cores, matching the capabilities of my host computer.
I also enabled hardware virtualisation support (Intel VT-x/EPT or AMD-V/RVI, depending on the processor), allowing Windows Server to make full use of the CPU's virtualisation features.
Network Adapter
For networking, I selected Bridged mode.
This configuration allows the virtual machine to appear as a separate device on my local network. Instead of sharing my computer's network connection behind Network Address Translation (NAT), the server receives its own IP address from the router, just like a physical server would. This made it much easier to test services such as Active Directory, DNS, DHCP, and file sharing across multiple virtual machines.
CD/DVD Drive
Finally, I configured the virtual DVD drive to use the Windows Server 2025 ISO that I had downloaded earlier.
With the installation media attached, the virtual machine was ready to boot directly into the Windows Server setup process.
After reviewing the hardware configuration, I clicked Close, followed by Finish, and VMware created the virtual machine.
At this point, the virtual machine existed, but Windows Server itself had not yet been installed.

Creating a Snapshot
Before powering on the virtual machine for the first time, I created a VMware snapshot.
Using VM → Snapshot → Take Snapshot, I saved the current state of the virtual machine and named it Before Install.
This snapshot provided a clean recovery point that I could return to at any time if the installation failed or if I wanted to restart the project without creating a brand-new virtual machine from scratch.
Although creating snapshots is optional, I found them incredibly useful throughout the project. They allowed me to experiment confidently, knowing I could always roll the virtual machine back to a known working state within minutes.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/3bb97711-3d19-4bb4-b225-b0f7285637bc" />

Section 2: Installing Windows Server 2025
With the virtual machine fully configured, I was ready for the most exciting part of the setup process installing Windows Server 2025. Although I was installing the operating system inside a virtual machine rather than on a physical computer, the installation experience was almost identical to installing Windows on a real PC. The only difference was that everything was being installed onto a virtual hard disk, allowing me to experiment safely without affecting my host operating system.

Step 1: Booting the Virtual Machine and Launching the Installer

13.  I powered on the virtual machine by clicking the green Play button in VMware.

14.  A few seconds later, the message "Press any key to boot from CD or DVD" appeared on the screen. I immediately pressed a key on my keyboard to boot from the Windows Server 2025 installation media. If this prompt is missed, simply restarting the virtual machine and trying again will bring it back.

15.  The Windows installer then began loading the required installation files from the ISO image. This process took a minute or two, during which I waited for the loading screen to complete before the installation wizard appeared.

Step 2: Configuring Language and Regional Settings

16.  On the first setup screen, I selected English (United States) as the installation language. You can choose a different language if it better suits your preference.

17.  For the Time and Currency Format, I selected English (Australian) because it aligned with my local time zone. If you're following this guide from another region, choose the option that best matches your location.

18.  Under Keyboard or Input Method, I kept the default US keyboard layout, as it is compatible with most standard keyboards. After confirming my selections, I clicked Next.

19.  On the next screen, I clicked Install Windows Server to begin the installation process.

Step 3: Selecting the Windows Server Edition

After the initial setup, I was presented with four different Windows Server 2025 editions. For this project, I selected Windows Server 2025 Standard Evaluation (Desktop Experience).

I specifically chose the Desktop Experience edition because it includes the full graphical user interface (GUI), complete with the familiar Windows desktop, Start menu, taskbar, File Explorer, and other graphical tools. The alternative editions, known as Server Core, do not include a graphical interface and are managed almost entirely through Command Prompt and PowerShell. While Server Core is commonly used in production environments due to its smaller footprint and improved security, I found the Desktop Experience much more suitable for learning and building my home lab, as it made navigation and server management significantly more intuitive.

Step 4: Accepting the Licence Agreement and Selecting the Installation Disk

13.  Before the installation could continue, I carefully reviewed the Microsoft licence terms and accepted them by ticking the appropriate checkbox.

14.  On the Installation Type screen, I selected Custom: Install Windows Server only (advanced). Since this was a brand-new virtual machine with no existing operating system, there was no need to perform an upgrade.

15.  The installer then displayed the available storage devices. I selected the 60 GB virtual hard disk that I had created during the virtual machine setup and clicked Next.

16.  Finally, I clicked Install to begin the installation process. Windows Server automatically copied the installation files, installed the operating system, and restarted the virtual machine several times. On my system, the entire installation took approximately 10 to 20 minutes to complete.

Note: If I were installing Windows Server on a physical computer, this step would permanently erase all data on the selected drive. However, because I was working inside a virtual machine, only the virtual hard disk was affected, making it a safe environment for learning and experimentation.

Step 5: Setting the Administrator Password

Once the installation was complete, Windows Server prompted me to create a password for the built-in Administrator account. This account has unrestricted administrative privileges and is used to manage every aspect of the server, so I ensured that the password was both strong and secure.

To improve security, I created a password that combined uppercase letters, lowercase letters, numbers, and special characters. After entering the password twice to confirm it, I pressed Enter to complete the setup.

Step 6: Logging In for the First Time

17.  At the Windows Server login screen, I used VMware's Send Ctrl+Alt+Delete option to display the sign-in prompt, as the standard keyboard shortcut is intercepted by the host operating system.

18.  I entered the Administrator password that I had just created and signed in.

19.  During the first login, Windows Server spent a few moments configuring my user profile and preparing the desktop environment. After this initial setup was complete, I was taken to the Windows Server 2025 desktop.

20.  Once the desktop loaded, a few introductory pop-up windows, such as those promoting Windows Admin Center and Azure Arc, appeared. Since they were not required for this project, I closed them by clicking the X button, leaving me with a clean desktop ready for the next stage of the server configuration.
21.  <img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d267f019-5fbf-45a6-8f43-c841e37c5af5" />

Section 3: First-Time Setup and Basic Configuration
With Windows Server 2025 successfully installed, my next step was to perform the initial server configuration. Before installing any server roles or additional features, I wanted to ensure the operating system was configured correctly and ready for the rest of the project.
There are a few essential tasks that I always complete on a newly installed Windows Server. These include configuring the correct time zone, assigning a static IP address, enabling Remote Desktop, and renaming the server. Completing these baseline configurations early helps create a stable environment and prevents configuration issues later, especially when deploying services such as Active Directory and DNS.

Step 1: Opening Server Manager
After logging into Windows Server for the first time, Server Manager launched automatically. This is the primary management console for Windows Server, providing a central location for configuring the server, installing roles and features, and monitoring its health.
Since I would be using Server Manager throughout this project, I pinned it to the taskbar for quick access.
From the navigation pane on the left, I selected Local Server. This page displays the server's current configuration, including the computer name, network settings, time zone, Remote Desktop status, and several other system properties. One feature I particularly appreciate is that almost every setting on this page can be modified simply by clicking its current value.

Step 2: Configuring the Correct Time Zone
Having the correct system time is important for authentication, logging, and communication between servers. To ensure the server reflected my local region, I updated the time zone before continuing with the rest of the configuration.
13.	In Local Server, I located the Time Zone field.
14.	I clicked the current time zone to open the Date and Time settings.
15.	Next, I selected Change time zone... and chose the time zone that matched my location.
16.	After clicking OK, the system clock updated immediately to reflect the correct local time.

Step 3: Disabling Internet Explorer Enhanced Security Configuration
Windows Server enables Internet Explorer Enhanced Security Configuration (IE ESC) by default. This feature is designed to improve security by restricting web browsing, but it can also make downloading software, drivers, and updates unnecessarily difficult during a lab setup.
Because this server was being used in a controlled home lab environment, I chose to disable the feature temporarily to make the configuration process smoother.

17.	In Local Server, I located IE Enhanced Security Configuration and clicked its current status.
18.	I changed the setting to Off for both Administrators and Users.
19.	Finally, I clicked OK to apply the changes.
Note: In a production environment, I would leave Internet Explorer Enhanced Security Configuration enabled because it provides an additional layer of protection against malicious websites. I disabled it here solely because this server was built for learning and testing purposes.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f2af487c-3726-442b-8d91-276a505cf9e8" />

Step 4: Assigning a Static IP Address
One of the most important configuration tasks was assigning the server a static IP address. Unlike client computers, servers should not rely on DHCP because their network address needs to remain consistent. Later in this project, I planned to install Active Directory Domain Services (AD DS) and DNS, both of which depend on clients always being able to locate the server using the same IP address.
20.  From Local Server, I clicked the Ethernet link to open the network settings.
21.  In the Network Connections window, I right-clicked the network adapter and selected Properties.
22.  I opened Internet Protocol Version 4 (TCP/IPv4) by double-clicking it.
23.  I selected Use the following IP address and entered the following network information:
·        IP Address: An available static IP address on my local network (for example, 192.168.1.222)
·        Subnet Mask: 255.255.255.0
·        Default Gateway: My router's IP address (typically 192.168.1.1)
·        Preferred DNS Server: 1.1.1.1 (Cloudflare). I planned to replace this later with the server's own IP address after installing the DNS Server role.
·        Alternate DNS Server: 8.8.8.8 (Google)
24.  After entering the required information, I clicked OK twice to save the configuration. As expected, the network connection disconnected briefly before reconnecting with the new settings.
25.  To confirm everything had been configured correctly, I reopened the adapter, selected Status, then Details, and verified that the IP address matched the one I had assigned.
Tip: I made a note of the server's static IP address because I would be using it repeatedly throughout the remainder of this project whenever another device needed to communicate with the server.

Step 5: Completing the Remaining Configuration with SConfig

Although Windows Server provides a graphical interface for most administrative tasks, Microsoft also includes a useful command-line utility called SConfig. I found this tool particularly helpful because it allows several common configuration tasks to be completed quickly from a simple numbered menu. It's especially useful when configuring multiple servers, as it helps maintain consistency across each installation.
To launch the tool, I opened Windows Terminal (Administrator) and entered the following command:

Sconfig
 
I then worked through the following configuration options:

Option

Configuration

Action Taken

2

Computer Name

Renamed the server to server01 and restarted the system when prompted.

4

Remote Management

Enabled remote management to allow administration from another computer if required.

5

Windows Update

Set updates to Manual so I could install updates at an appropriate time during the project.

7

Remote Desktop

Enabled Remote Desktop and selected the option that allows connections from any version of Remote Desktop, which was sufficient for this lab environment.
After renaming the server, Windows Server restarted automatically. Once it had rebooted, I logged back in and returned to Local Server to verify that the computer name and all of the configuration changes had been applied successfully.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/42906a55-c916-40de-8dec-1ba9d9393d22" />

Section 4: Understanding Roles and Features
Before I began installing any services on my Windows Server, I wanted to understand the difference between Roles and Features. Taking the time to learn this made the rest of the project much easier because it helped me understand how Windows Server is designed and why certain components need to be installed.
I realised that a Windows Server doesn't automatically perform every possible function. Instead, it starts as a general-purpose operating system, and I decide what responsibilities it should take on by installing the appropriate server roles.

What Is a Server Role?
A server role defines the primary function or responsibility of a server. In simple terms, it answers the question:
"What is this server supposed to do?"
When I install a role, I'm assigning a specific job to the server. As my project progresses, the server will gradually take on multiple responsibilities depending on the services I install.
Some of the most common server roles include:
•	Active Directory Domain Services (AD DS): Allows the server to manage users, computers, groups, and authentication across an entire Windows domain.
•	DNS Server: Translates computer names into IP addresses, making it possible for devices on the network to locate and communicate with one another.
•	DHCP Server: Automatically assigns IP addresses and other network settings to devices when they connect to the network.
•	File and Storage Services: Enables the server to provide shared folders, manage storage, and control access to files across the network.
•	Hyper-V: Converts the server into a virtualization host capable of creating and running multiple virtual machines.
•	Remote Desktop Services: Allows users to access and use the server remotely as though they were physically sitting in front of it.
Each of these roles serves a different purpose, and depending on the needs of an organisation, a server may host one role or several.
Understanding Features
While roles determine what a server does, features provide additional tools and functionality that support those roles.
I like to think of features as optional enhancements. They don't usually define the server's primary purpose, but they make administration easier, improve security, or add extra capabilities.
Some common Windows Server features include:
•	Group Policy Management – Used to create and manage Group Policy Objects (GPOs) across an Active Directory environment.
•	Windows Server Backup – Enables scheduled backups and recovery of server data.
•	BitLocker Drive Encryption – Protects data by encrypting entire disk volumes.
•	Failover Clustering – Provides high availability by allowing multiple servers to work together so services remain available if one server fails.
Throughout this project, I installed both roles and features using the Add Roles and Features Wizard in Server Manager. Since many server services depend on one another, I found myself returning to this wizard several times as I expanded the functionality of my server.
Naming Servers in Professional Environments
Another concept I found useful was learning how servers are named in real-world organisations.
Rather than using random names such as planets, animals, or fictional characters, most organisations follow a structured naming convention that immediately identifies a server's purpose and, in many cases, its location. This makes server management and troubleshooting much easier, especially in environments with dozens or even hundreds of servers.

Some examples include:
•	DMT-MEL-AD01 → Daniel Mitchell Training, Melbourne, Active Directory, Server 01
•	DMT-SYD-FS01 → Daniel Mitchell Training, Sydney, File Server, Server 01
•	CORP-DC01 → Corporate, Domain Controller, Server 01
Seeing examples like these helped me appreciate why consistent naming conventions are so important. If an issue occurs, the server's name immediately tells an administrator what its role is and where it belongs within the organisation.
For my own home lab, I kept the naming simple and practical by using names such as server01 and server02. Although my environment was much smaller than a production network, following a consistent naming convention helped me develop habits that align with industry best practices.
 
Section 3: First-Time Setup and Basic Configuration
With Windows Server 2025 successfully installed, my next step was to perform the initial server configuration. Before installing any server roles or additional features, I wanted to ensure the operating system was configured correctly and ready for the rest of the project.
There are a few essential tasks that I always complete on a newly installed Windows Server. These include configuring the correct time zone, assigning a static IP address, enabling Remote Desktop, and renaming the server. Completing these baseline configurations early helps create a stable environment and prevents configuration issues later, especially when deploying services such as Active Directory and DNS.

Step 1: Opening Server Manager
After logging into Windows Server for the first time, Server Manager launched automatically. This is the primary management console for Windows Server, providing a central location for configuring the server, installing roles and features, and monitoring its health.
Since I would be using Server Manager throughout this project, I pinned it to the taskbar for quick access.
From the navigation pane on the left, I selected Local Server. This page displays the server's current configuration, including the computer name, network settings, time zone, Remote Desktop status, and several other system properties. One feature I particularly appreciate is that almost every setting on this page can be modified simply by clicking its current value.

Step 2: Configuring the Correct Time Zone
Having the correct system time is important for authentication, logging, and communication between servers. To ensure the server reflected my local region, I updated the time zone before continuing with the rest of the configuration.
13.	In Local Server, I located the Time Zone field.
14.	I clicked the current time zone to open the Date and Time settings.
15.	Next, I selected Change time zone... and chose the time zone that matched my location.
16.	After clicking OK, the system clock updated immediately to reflect the correct local time.

Step 3: Disabling Internet Explorer Enhanced Security Configuration
Windows Server enables Internet Explorer Enhanced Security Configuration (IE ESC) by default. This feature is designed to improve security by restricting web browsing, but it can also make downloading software, drivers, and updates unnecessarily difficult during a lab setup.
Because this server was being used in a controlled home lab environment, I chose to disable the feature temporarily to make the configuration process smoother.
17.	In Local Server, I located IE Enhanced Security Configuration and clicked its current status.
18.	I changed the setting to Off for both Administrators and Users.
19.	Finally, I clicked OK to apply the changes.
Note: In a production environment, I would leave Internet Explorer Enhanced Security Configuration enabled because it provides an additional layer of protection against malicious websites. I disabled it here solely because this server was built for learning and testing purposes.

Step 4: Assigning a Static IP Address
One of the most important configuration tasks was assigning the server a static IP address. Unlike client computers, servers should not rely on DHCP because their network address needs to remain consistent. Later in this project, I planned to install Active Directory Domain Services (AD DS) and DNS, both of which depend on clients always being able to locate the server using the same IP address.
20.	From Local Server, I clicked the Ethernet link to open the network settings.
21.	In the Network Connections window, I right-clicked the network adapter and selected Properties.
22.	I opened Internet Protocol Version 4 (TCP/IPv4) by double-clicking it.
23.	I selected Use the following IP address and entered the following network information:
•	IP Address: An available static IP address on my local network (for example, 192.168.1.222)
•	Subnet Mask: 255.255.255.0
•	Default Gateway: My router's IP address (typically 192.168.1.1)
•	Preferred DNS Server: 1.1.1.1 (Cloudflare). I planned to replace this later with the server's own IP address after installing the DNS Server role.
•	Alternate DNS Server: 8.8.8.8 (Google)
24.	After entering the required information, I clicked OK twice to save the configuration. As expected, the network connection disconnected briefly before reconnecting with the new settings.
25.	To confirm everything had been configured correctly, I reopened the adapter, selected Status, then Details, and verified that the IP address matched the one I had assigned.
Tip: I made a note of the server's static IP address because I would be using it repeatedly throughout the remainder of this project whenever another device needed to communicate with the server.

Step 5: Completing the Remaining Configuration with SConfig
Although Windows Server provides a graphical interface for most administrative tasks, Microsoft also includes a useful command-line utility called SConfig. I found this tool particularly helpful because it allows several common configuration tasks to be completed quickly from a simple numbered menu. It's especially useful when configuring multiple servers, as it helps maintain consistency across each installation.
To launch the tool, I opened Windows Terminal (Administrator) and entered the following command:
sconfig
I then worked through the following configuration options:
Option	Configuration	Action Taken
2	Computer Name	Renamed the server to server01 and restarted the system when prompted.
4	Remote Management	Enabled remote management to allow administration from another computer if required.
5	Windows Update	Set updates to Manual so I could install updates at an appropriate time during the project.
7	Remote Desktop	Enabled Remote Desktop and selected the option that allows connections from any version of Remote Desktop, which was sufficient for this lab environment.
After renaming the server, Windows Server restarted automatically. Once it had rebooted, I logged back in and returned to Local Server to verify that the computer name and all of the configuration changes had been applied successfully.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/a3c9d746-b42c-4d7e-b57e-d2bc5cc133cb" />

Section 6: Creating DNS Records
With the DNS Server role installed and configured, I wanted to gain a deeper understanding of how DNS records are created and managed manually. Although Windows automatically creates many DNS records—for example, when a computer joins the domain or when certain server roles are installed—I knew there would be situations where I would need to create records myself. Learning how to do this manually would help me troubleshoot DNS issues more effectively and give me greater control over my network environment.
Opening DNS Manager
13.	I opened Server Manager, selected Tools, and then clicked DNS to launch the DNS Manager console.
14.	In the left-hand pane, I expanded my server name and then expanded Forward Lookup Zones.
15.	Next, I selected my domain's forward lookup zone (corp.danieltraining.com, or whichever domain name was used during the setup).
Inside this zone, I could already see several DNS records that had been created automatically by Active Directory. One of these was an A (Host) record for server01, which mapped the server's hostname to its IP address. Seeing these automatically generated records helped me understand how closely Active Directory and DNS work together.
Creating an A Record (Hostname to IP Address)
The first type of DNS record I created was an A (Host) record. An A record maps a hostname directly to an IPv4 address and is the most commonly used DNS record in Windows environments.
16.	Inside the forward lookup zone, I right-clicked an empty area and selected New Host (A or AAAA).
17.	In the Name field, I entered router.
18.	In the IP Address field, I entered the IP address of my network's default gateway (for example, 192.168.1.1).
19.	For this initial example, I left Create associated pointer (PTR) record unchecked and clicked Add Host.
Once the record had been created, I wanted to confirm that it was working correctly. I opened Command Prompt and ran the following command:
ping router
The command successfully resolved router to router.corp.danieltraining.com before translating it to the IP address that I had assigned. This confirmed that the newly created A record was functioning as expected.
Creating a CNAME Record (Alias)
Next, I created a CNAME (Canonical Name) record. Unlike an A record, which points directly to an IP address, a CNAME record creates an alias that points to another hostname. This is particularly useful when multiple services need user-friendly names without requiring duplicate A records.
20.	I right-clicked inside the forward lookup zone and selected New Alias (CNAME).
21.	In the Alias name field, I entered gateway.
22.	In the Fully qualified domain name (FQDN) for target host field, I entered router.corp.danieltraining.com, which was the A record I had just created.
23.	After confirming the information, I clicked OK.
To verify that the alias was working, I opened Command Prompt and executed:
ping gateway
The result showed that gateway first resolved to router.corp.danieltraining.com, which then resolved to the correct IP address. This demonstrated how a CNAME record acts as an alias rather than storing an IP address itself.
Creating a Reverse Lookup Zone and PTR Record
After creating forward lookup records, I moved on to configuring reverse DNS.
A PTR (Pointer) record performs the opposite function of an A record. Instead of translating a hostname into an IP address, it translates an IP address back into a hostname. Reverse lookups are commonly used by email servers, security applications, monitoring tools, and troubleshooting utilities to verify the identity of devices on a network.
Before creating a PTR record, I first needed to create a Reverse Lookup Zone.
24.	In DNS Manager, I right-clicked Reverse Lookup Zones and selected New Zone.
25.	I selected Primary Zone, ensured Store the zone in Active Directory was enabled, and clicked Next.
26.	I chose the option to replicate the zone to all DNS servers running on domain controllers in this domain, then clicked Next.
27.	I selected IPv4 Reverse Lookup Zone and continued.
28.	For the Network ID, I entered the first three octets of my network address (for example, 192.168.1) before clicking Next.
29.	Finally, I selected Allow only secure dynamic updates, clicked Next, and then Finish to create the reverse lookup zone.
Creating a PTR Record
With the reverse lookup zone in place, I recreated the router A record so that Windows would automatically generate the corresponding PTR record.
30.	Inside the forward lookup zone, I deleted the previous router A record.
31.	I created a new Host (A) record using the same details:
•	Name: router
•	IP Address: 192.168.1.1
32.	This time, I selected Create associated pointer (PTR) record before continuing.
33.	I clicked Add Host to create both records simultaneously.
To verify that reverse DNS was working correctly, I opened Command Prompt and ran:
nslookup 192.168.1.1
This time, the command successfully resolved the IP address back to router.corp.danieltraining.com, confirming that the PTR record had been created correctly and that reverse name resolution was functioning as expected.
Completing these exercises gave me a much better understanding of how forward and reverse DNS work together. Although Windows automatically manages many DNS records in an Active Directory environment, knowing how to create and verify A, CNAME, and PTR records manually is an essential skill for troubleshooting network connectivity and administering Windows Server environments.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/09144be3-49f4-49cd-a561-e855915b4f98" />

Section 7: Creating Organisational Units, Users, and Security Groups
With my domain controller fully configured and DNS working as expected, the next step was to begin building the Active Directory environment. At this stage, the domain was essentially empty, so I needed to create the structure that would eventually hold all of my users, groups, and computers.
This was one of the most interesting parts of the project because it closely reflects the kind of work carried out by IT Support Engineers and System Administrators on a daily basis. Every organisation relies on Active Directory to organise its users, manage permissions, and control access to resources, so understanding how these objects fit together is a fundamental skill.
Opening Active Directory Users and Computers
13.	I opened Server Manager, selected Tools, and then clicked Active Directory Users and Computers.
14.	Once the console opened, I expanded my domain (corp.danieltraining.com) from the left-hand pane. Windows had already created several default containers, which serve different purposes within Active Directory. Rather than modifying these default containers, I decided to create my own organisational structure to keep everything organised and easier to manage.

Step 1: Creating Organisational Units (OUs)
One of the first things I learned about Active Directory is that Organisational Units (OUs) work much like folders. They allow administrators to organise users, computers, and groups into a logical structure.
More importantly, OUs make it possible to apply Group Policy. Any policy linked to an OU automatically affects every object stored inside it, making administration much simpler as the environment grows.
To build my Active Directory structure, I created the following OUs:
15.	I right-clicked corp.danieltraining.com, selected New, and then Organisational Unit.
16.	I named the first OU Daniel Training, which served as the root container for everything else. I left Protect container from accidental deletion enabled before clicking OK.
17.	Inside the Daniel Training OU, I created another OU named End Users. This would hold all of the user accounts in my lab.
18.	I then created a second OU called Security Groups, where I planned to store all of my Active Directory security groups.
19.	Finally, I created another OU named Servers, which I would later use to organise any Windows Servers that joined the domain.
Although this structure is fairly simple, it mirrors how many organisations organise their Active Directory environments.
Note: There is no single "correct" way to structure OUs. Some organisations organise them by department (such as HR, Finance, and IT), while others organise them by office location or business function. What matters most is choosing a structure that is logical, consistent, and easy to manage as the environment grows.

Step 2: Creating User Accounts
With the organisational structure in place, I moved on to creating user accounts.
20.	I selected the End Users OU.
21.	Inside the OU, I right-clicked and selected New > User.
For my first account, I entered the following information:
•	First Name: Emma
•	Last Name: Johnson
•	User Logon Name: emma.johnson@corp.danieltraining.com
22.	After clicking Next, I created a temporary password (Welcome@123) and enabled the option User must change password at next logon before completing the wizard.
I repeated the same process to create two additional users:
•	James Smith — james.smith@corp.danieltraining.com
•	Sarah White — sarah.white@corp.danieltraining.com
Creating multiple users helped make my lab feel more like a real business environment instead of a domain with only a single administrator account.
Why require a password change?
In a real organisation, administrators usually assign a temporary password when creating a new account. By enabling User must change password at next logon, the user creates their own password the first time they sign in. This improves security because administrators never need to know or store a user's permanent password.

Step 3: Adding Additional User Information
Creating the user account is only part of the process. In many organisations, administrators also maintain additional information about each employee inside Active Directory.
To explore these options, I opened the properties of James Smith by double-clicking his account and completed several of the available fields.
Within the General tab, I added a brief description identifying James as an IT Administrator, along with an office location and telephone number.
Under the Account tab, I noted that an account expiry date can be configured. This is particularly useful for contractors, temporary staff, or interns whose accounts should automatically become inactive after a specific date.
In the Organisation tab, I entered details such as the user's job title, department, company name, and reporting manager. For this exercise, I assigned Emma Johnson as James's manager.
Finally, I looked at the Member Of tab, which displays the security groups that a user belongs to. At this stage, the list was empty because I had not yet created or assigned any security groups.

Step 4: Creating Security Groups
Once my user accounts were in place, the next task was to create security groups.
Security groups allow administrators to assign permissions to a group rather than to individual users. This approach makes managing access much easier, especially in larger organisations. Instead of granting permissions to every employee individually, administrators simply add users to the appropriate group.
23.	I selected the Security Groups OU.
24.	I right-clicked inside the OU and selected New > Group.
25.	I created my first security group with the following settings:
•	Group Name: Air Staff
•	Group Scope: Global
•	Group Type: Security
After confirming the settings, I clicked OK.
26.	I repeated the process to create another security group named IT Admins, using the same configuration.
Step 5: Adding Users to Security Groups
With both groups created, I assigned users to them using two different methods. Learning both approaches gave me a better understanding of how Active Directory can be managed.
Method A: Adding Members from the Group
27.	I opened the IT Admins group by double-clicking it.
28.	Under the Members tab, I clicked Add.
29.	I searched for James, selected James Smith, and confirmed the selection.
James was now a member of the IT Admins security group.
Method B: Adding Groups from the User Account
30.	Next, I opened the properties of Sarah White.
31.	I selected the Member Of tab and clicked Add.
32.	I searched for Air Staff, selected the group, clicked OK, and then Apply to save the changes.
After completing these tasks, my Active Directory environment contained a realistic structure that closely resembled what I might encounter in a professional workplace.
•	James Smith belonged to the IT Admins group.
•	Sarah White belonged to the Air Staff group.
•	Emma Johnson had not yet been assigned to any security group.
Although this was a relatively small environment, creating these organisational units, user accounts, and security groups helped me understand how businesses organise their Active Directory infrastructure. More importantly, it laid the foundation for the next stages of the project, where these users and groups would be used to manage permissions, apply Group Policies, and control access to shared resources.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/b748f9e4-cbb9-4392-becf-da66fe7d595b" />

Section 8: Joining a Second Server to the Domain
Up to this point, my lab consisted of a single Windows Server acting as the Domain Controller. While this was enough to learn the basics of Active Directory, I knew that real-world environments rarely rely on just one server. Different servers are typically assigned different responsibilities to improve performance, security, and manageability.
To make my lab more realistic, I created a second Windows Server virtual machine (server02) and joined it to the domain I had already created on server01. This would allow me to assign additional roles to server02 later in the project, such as configuring it as a dedicated file server.
Step 1: Preparing Server02
To begin, I created a second virtual machine in VMware using the same process I followed in Section 1. I then installed Windows Server 2025 using the same installation steps covered in Section 2.
During the initial configuration, I made a few important changes so that the second server would be unique on the network:
•	Computer Name: server02
•	Static IP Address: A different address on the same network (for example, 192.168.1.223)
•	Preferred DNS Server: 192.168.1.222, which is the IP address of server01
One of the most important lessons I learned during this stage was that a computer joining an Active Directory domain must use the domain controller as its preferred DNS server.
Initially, it might seem reasonable to use public DNS servers such as Cloudflare (1.1.1.1) or Google (8.8.8.8), but doing so would prevent the domain join from working. During the join process, Windows relies on DNS to locate the Domain Controller. If the DNS server cannot resolve the domain, the computer simply won't be able to find it.
Important: Before attempting to join any Windows computer or server to an Active Directory domain, always ensure that its preferred DNS server points to the Domain Controller.
Step 2: Verifying DNS Connectivity
Before attempting the domain join, I wanted to confirm that server02 could successfully communicate with the Domain Controller through DNS.
To do this, I opened Command Prompt on server02 and ran the following command:
ping server01.corp.danieltraining.com
The hostname successfully resolved to the IP address of server01, and I received reply packets. This confirmed that DNS was configured correctly and that server02 could locate the Domain Controller on the network.
Step 3: Joining Server02 to the Domain
With DNS confirmed to be working, I proceeded to join the server to my Active Directory domain.
13.	In Server Manager, I selected Local Server and clicked the Workgroup link.
14.	This opened the System Properties window, where I clicked Change.
15.	Under Member of, I selected Domain and entered the name of my domain:
corp.danieltraining.com
16.	After clicking OK, Windows prompted me for credentials. I entered the domain administrator account:
•	Username: corp\administrator
•	Password: The password created for the domain Administrator account.
17.	After a few moments, Windows displayed the message:
"Welcome to the corp.danieltraining.com domain."
Seeing this confirmation was a satisfying milestone because it meant the server had successfully joined the Active Directory domain.
18.	I clicked OK, acknowledged the restart prompt, and restarted server02 to complete the process.
Step 4: Verifying the Domain Join from Server01
After the restart, I switched back to server01 to confirm that the new server had been added to Active Directory.
19.	I opened Active Directory Users and Computers from Server Manager.
20.	Under my domain, I selected the default Computers container, where I found server02 listed as a newly joined computer object.
21.	To keep my Active Directory organised, I moved server02 from the default Computers container into the Servers organisational unit that I had created earlier.
Although this wasn't strictly required, organising computer accounts into dedicated OUs is considered good administrative practice because it makes applying Group Policies and managing systems much easier as the environment grows.
Step 5: Logging In Using Domain Credentials
Once server02 restarted, I signed in using the domain administrator account rather than the local Administrator account.
22.	On the Windows login screen, I selected Other User.
23.	I signed in using:
•	Username: corp\administrator
•	Password: My domain administrator password.
24.	After logging in, I opened Server Manager and selected Local Server. This time, instead of displaying WORKGROUP, the server showed that it was a member of corp.danieltraining.com, confirming that the domain join had been completed successfully.
At this point, my lab had evolved from a standalone server into a small Windows domain consisting of two connected servers. From here, I could centrally manage server02 from server01, apply Group Policies, install additional server roles, and manage both systems through Active Directory.
Although my environment only contained two servers, the setup closely reflected how many enterprise networks are structured, where different servers perform different roles while remaining centrally managed within the same Active Directory domain. This milestone laid the foundation for the remaining stages of the project, where I would continue expanding the capabilities of my Windows Server environment.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d4e9892f-395a-4d2c-9c68-be5642b08fa4" />

Section 9: Group Policy Objects (GPOs)
As I continued building my Windows Server environment, I reached one of the features that truly demonstrates the power of Active Directory—Group Policy. At first, Group Policy seemed overwhelming because of the sheer number of settings available. However, once I understood how it works, it quickly became one of my favourite administrative tools.
At its core, Group Policy allows me to configure a setting once and automatically apply it to multiple users or computers across the network. Instead of configuring every device individually, I can create a single policy, link it to the appropriate location in Active Directory, and let Windows handle the rest.
The example that really helped me understand its value was software deployment. Imagine an organisation with 800 employee computers, all of which need a PDF reader installed. Without Group Policy, an administrator would have to visit each computer individually or create a separate deployment process. With Group Policy, I only need to configure the software deployment once and link the policy to the correct Organisational Unit (OU). The next time those computers refresh their policies, the software is installed automatically.
What could easily take several days can be reduced to just a few minutes of administration.
Understanding How Group Policy Is Applied
One of the first concepts I learned was that Group Policy follows a specific hierarchy. Policies can be applied at different levels within Active Directory, and Windows processes them in a particular order known as LSDOU:
•	Local – Policies configured directly on an individual computer.
•	Site – Policies applied to an Active Directory site, usually representing a geographical location.
•	Domain – Policies that affect every user and computer within the domain.
•	Organisational Unit (OU) – Policies targeted at specific users or computers within a particular OU.
Understanding this order helped me see why some policies override others. If two policies conflict, the one processed last generally takes precedence. This means that a policy linked to an OU can override a setting applied at the domain level.
I also learned that administrators can Enforce a Group Policy Object (GPO), preventing lower-level OUs from overriding its settings. This is particularly useful for organisation-wide security policies that must always remain in effect.
Step 1: Opening Group Policy Management
13.	I opened Server Manager, selected Tools, and then clicked Group Policy Management.
14.	Inside the console, I expanded my forest and then expanded my domain (corp.danieltraining.com).
15.	I noticed that Windows had already created and linked a Default Domain Policy at the domain level. Before creating my own policies, I opened it and explored the Settings tab to understand what Microsoft configures by default.
Exploring the Default Domain Policy
The Default Domain Policy provides a baseline level of security for every computer and user in the domain. While reviewing it, I noticed several important settings already configured, including:
•	Passwords expire after 42 days by default.
•	The minimum password length is 7 characters.
•	Password complexity is enabled, requiring a combination of uppercase letters, lowercase letters, numbers, or special characters.
•	User accounts can be locked after a number of failed sign-in attempts, helping protect against password-guessing attacks.
For my lab, I left these default settings unchanged. However, I also recognised that most production environments would enforce stronger password policies, such as increasing the minimum password length to at least 12 characters and applying stricter security requirements.
Step 2: Creating a Custom Group Policy Object
After exploring the default policy, I wanted to create my own Group Policy Object so I could see how policies are created, linked, and managed.
For this exercise, I created a policy that prevents end users from accessing the Control Panel and PC Settings. Restricting access to these settings helps prevent users from making unauthorised system changes.
16.	I right-clicked the End Users organisational unit and selected Create a GPO in this domain, and Link it here.
17.	I named the policy Disable Control Panel - End Users and clicked OK.
18.	After creating the policy, I right-clicked it and selected Edit to open the Group Policy Management Editor.
19.	Inside the editor, I navigated to:
User Configuration → Policies → Administrative Templates → Control Panel
20.	I opened the policy named Prohibit access to Control Panel and PC Settings.
21.	I changed the policy setting to Enabled. Before applying the change, I took a moment to read Microsoft's explanation, which clearly described the effect of enabling the policy.
22.	Finally, I clicked Apply and then OK to save the configuration.
At this point, the policy was complete. Any user account located within the End Users organisational unit would receive this setting the next time Group Policy refreshed or the next time the user signed in. Once applied, the Control Panel and PC Settings would no longer be accessible to those users.
Seeing this in action helped me appreciate how powerful Group Policy can be. Rather than configuring restrictions on each computer individually, I only needed to configure the policy once.
Step 3: Forcing an Immediate Group Policy Update
Normally, Windows refreshes Group Policy automatically at regular intervals. However, while testing new policies, I didn't want to wait.
Instead, I opened Command Prompt on a domain-joined computer and ran the following command:
gpupdate /force
This command immediately downloads and applies the latest Group Policy settings from the Domain Controller, making it an invaluable troubleshooting tool whenever changes don't appear to take effect straight away.
Step 4: Checking the FSMO Roles
As I learned more about Active Directory, I discovered that every domain relies on five specialised roles known as Flexible Single Master Operations (FSMO) roles.
These roles perform critical tasks such as managing schema updates, allocating relative identifiers (RIDs), and ensuring consistency across the domain. Although multiple Domain Controllers can exist within the same environment, each FSMO role can only be held by one Domain Controller at a time.
To identify which server owned these roles, I ran the following command:
netdom query fsmo
Because my lab contained only one Domain Controller (server01), all five FSMO roles were assigned to that server. In larger enterprise environments, these roles are often distributed across multiple Domain Controllers to improve resilience and simplify maintenance.
Learning about FSMO roles gave me a much better understanding of how Active Directory functions behind the scenes. It also reinforced the importance of documenting infrastructure carefully. During an outage or disaster recovery scenario is the worst possible time to discover that you don't know which server holds your critical Active Directory roles.
By the end of this section, I felt much more confident using Group Policy. I had learned how policies are processed, explored the default security settings that Windows applies automatically, created and linked my own custom GPO, forced policy updates for testing, and identified the FSMO role holders within my domain. These are all tasks that form part of the day-to-day responsibilities of Windows Server administrators and IT support professionals.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/e83cedbd-09f7-4416-bc6a-3b9e540b3af6" />

Section 10: Installing and Configuring DHCP
After configuring Active Directory and DNS, the next service I needed to set up was Dynamic Host Configuration Protocol (DHCP). DHCP is responsible for automatically assigning IP addresses and other network settings to devices as they connect to the network.
Without DHCP, every computer, laptop, printer, or other network device would have to be configured with a static IP address manually. While that might be manageable for a handful of devices, it quickly becomes impractical in larger environments. DHCP removes that burden by automatically providing each device with the correct network configuration, making network administration far more efficient.
For this stage of my lab, I installed the DHCP Server role on server01 and configured it to distribute IP addresses to devices connected to my network.
Prerequisites
Before installing DHCP, I made sure the following requirements had already been met:
•	server01 had been assigned a static IP address (completed in Section 3).
•	server01 had already been promoted to a Domain Controller and joined to the Active Directory domain.
•	I was signed in using the Domain Administrator account.
With these prerequisites in place, I was ready to install and configure the DHCP service.
Step 1: Installing the DHCP Server Role
13.	I opened Server Manager, selected Manage, and clicked Add Roles and Features.
14.	I clicked Next through the initial pages of the wizard, leaving Role-based or feature-based installation selected and ensuring that server01 was the chosen destination server.
15.	From the list of available server roles, I selected DHCP Server.
16.	When Windows prompted me to install the required supporting features, I clicked Add Features.
17.	I continued through the remaining pages of the wizard, accepting the default settings until I reached the confirmation screen.
18.	On the Confirmation page, I enabled Restart the destination server automatically if required.
19.	I clicked Install and waited while Windows installed the DHCP Server role.
20.	Once the installation finished, I noticed a yellow notification flag in Server Manager. I clicked it and selected Complete DHCP Configuration.
21.	The post-installation wizard guided me through the final configuration. After clicking Next, I selected Commit, which authorised the DHCP server within Active Directory.
This authorisation step is an important security feature. In an Active Directory environment, only authorised DHCP servers are allowed to lease IP addresses. This prevents unauthorised or "rogue" DHCP servers from distributing incorrect network settings that could disrupt the entire network.
After the authorisation process completed successfully, I closed the wizard.
Step 2: Creating a DHCP Scope
With the DHCP service installed, I needed to create a scope.
A DHCP scope defines the pool of IP addresses that the server is allowed to assign to client devices. It also contains important network information such as the default gateway, subnet mask, and DNS server that clients will automatically receive.
22.	I opened Server Manager, selected Tools, and launched DHCP.
23.	Inside DHCP Manager, I expanded my server and selected IPv4.
24.	I right-clicked IPv4 and selected New Scope.
25.	When the New Scope Wizard opened, I gave the scope a descriptive name:
LAN Network – Lab Environment
I then clicked Next.
26.	Next, I configured the range of IP addresses that DHCP could assign:
•	Start IP Address: 192.168.1.5
•	End IP Address: 192.168.1.99
•	Subnet Mask: 255.255.255.0 (/24)
27.	The wizard then prompted me to configure any exclusions. Exclusions are addresses within the scope that DHCP should never assign, usually because they are reserved for devices using static IP addresses.
For this lab, I could have excluded addresses such as 192.168.1.1 to 192.168.1.4, which are commonly reserved for routers, servers, or network equipment. Since my static devices already used addresses outside my DHCP range, I left the exclusion list unchanged and continued.
28.	I kept the default lease duration of 8 days, which determines how long a client keeps its assigned IP address before renewing it.
29.	When asked whether I wanted to configure DHCP options immediately, I selected Yes.
30.	I entered my router's IP address (192.168.1.1) as the Default Gateway, clicked Add, and continued.
31.	The wizard automatically detected my Active Directory domain and DNS server settings. I confirmed that the domain name was correct and verified that the DNS server pointed to server01 (192.168.1.222) before clicking Next.
32.	Since I wasn't using Windows Internet Name Service (WINS) in my environment, I skipped that section.
33.	Finally, I chose Activate this scope now and completed the wizard by clicking Finish.
With that, my DHCP server was fully configured and ready to begin assigning IP addresses to client devices.
Step 3: Verifying DHCP Functionality
To confirm that DHCP was working correctly, I tested it from a client computer connected to the network.
Rather than waiting for the client to request a new address automatically, I forced it to obtain a fresh DHCP lease using the following commands:
ipconfig /release
ipconfig /renew
ipconfig /all
The first command released the computer's existing IP address, the second requested a new lease from the DHCP server, and the final command displayed the complete network configuration.
When I reviewed the output, I confirmed that:
•	The client had received an IP address within my configured range (192.168.1.5 – 192.168.1.99).
•	The Default Gateway was correctly set to my router (192.168.1.1).
•	The DNS Server was pointing to server01, allowing the client to locate resources within the Active Directory domain.
This confirmed that DHCP was functioning exactly as expected.
Monitoring DHCP Leases
One feature I found particularly useful was the ability to monitor DHCP activity directly from DHCP Manager.
By expanding the DHCP scope, I could view several useful sections:
•	Address Pool – Displays the range of IP addresses available for assignment.
•	Address Leases – Shows every device that has received an IP address from the DHCP server, including its hostname, assigned IP address, and lease expiration time.
•	Reservations – Allows me to permanently reserve a specific IP address for a particular device using its MAC address.
Reservations are especially useful for devices that should always keep the same IP address, such as printers, network switches, wireless access points, or other infrastructure devices. They provide the convenience of DHCP while ensuring that important devices always receive the same IP address whenever they reconnect to the network.
Completing this section gave me a much better understanding of how DHCP works alongside Active Directory and DNS. Together, these three services form the foundation of most Windows enterprise networks. With DHCP automatically assigning network settings, DNS resolving device names, and Active Directory managing authentication, my lab environment was beginning to resemble the infrastructure commonly found in real-world organisations.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ad362663-f931-4036-802d-7c416deb3103" />

Section 11: Storage Spaces. Creating Storage Pools and Virtual Disks
As I continued building my Windows Server lab, I wanted to explore one of Windows Server's built-in storage technologies: Storage Spaces. This feature allows multiple physical or virtual disks to be combined into a single storage pool, from which virtual disks can be created with different levels of redundancy and fault tolerance.
One of the things I found most interesting about Storage Spaces is that it provides functionality similar to a traditional hardware RAID controller, but entirely through software. This means I can build resilient storage without requiring any additional hardware, making it an excellent solution for home labs and small environments.
For this exercise, I added five virtual hard disks to server01, combined them into a storage pool, created a parity virtual disk (similar to RAID 5), and formatted it as a usable volume.
Step 1: Adding Virtual Disks in VMware
Before I could configure Storage Spaces, I first needed to provide Windows Server with additional storage.
13.	I shut down server01, right-clicked the virtual machine in VMware, and selected Settings.
14.	From the hardware settings window, I clicked Add, selected Hard Disk, and continued through the wizard.
15.	I created a new virtual hard disk with a capacity of 10 GB.
16.	I selected Store virtual disk as a single file and left Allocate all disk space now unchecked. This enables thin provisioning, meaning the virtual disk only consumes storage on the host computer as data is written to it instead of reserving the entire 10 GB immediately.
17.	After accepting the default file name, I clicked Finish.
18.	I repeated the same process four more times until server01 had a total of five additional 10 GB virtual hard disks.
19.	Once all five disks had been added, I clicked OK and powered the virtual machine back on.
Note: In a production environment, these would typically be physical hard drives or SSDs installed inside a server. In my lab, they were virtual disks, but Windows Server manages them in exactly the same way, making this an excellent way to learn Storage Spaces without additional hardware.
Step 2: Bringing the Disks Online
After Windows Server started, the newly added disks were detected automatically, but they were not yet ready for use.
20.	I opened Server Manager and selected File and Storage Services.
21.	Under Disks, I located the five newly added drives. As expected, they all appeared as Offline and Unallocated.
22.	I right-clicked each disk individually and selected Bring Online, confirming the warning message when prompted.
23.	After bringing each disk online, I initialised them by right-clicking each drive and selecting Initialize. I chose the GPT (GUID Partition Table) partition style for every disk.
At this point, all five disks were available for Storage Spaces to use.
Step 3: Creating a Storage Pool
With the disks prepared, I was ready to create my first storage pool.
24.	Within File and Storage Services, I selected Storage Pools.
25.	From the Tasks menu, I chose New Storage Pool.
26.	The wizard detected server01 as the available storage subsystem. I selected it and clicked Next.
27.	I named the pool Lab Storage Pool.
28.	I selected all five 10 GB disks and left the allocation setting as Automatic before continuing.
29.	Before creating the pool, I reviewed the summary page, which showed a total storage capacity of approximately 50 GB. Everything looked correct, so I clicked Create.
The storage pool was created successfully and was now ready to host one or more virtual disks.
Step 4: Creating a Virtual Disk
Immediately after creating the storage pool, Windows prompted me to create a virtual disk.
30.	I left the Create a virtual disk when this wizard closes option enabled and continued.
31.	I named the virtual disk Lab Virtual Disk.
32.	On the Storage Layout page, I selected Parity.
I chose the Parity layout because it provides fault tolerance similar to RAID 5. Instead of storing every file on a single disk, Windows distributes both data and parity information across all the disks in the pool. This means that if one disk fails, the data can still be reconstructed from the remaining disks.
33.	For the provisioning method, I selected Thin Provisioning. This allows the virtual disk to grow as more data is written instead of allocating all available storage immediately.
34.	Finally, I specified a virtual disk size of 10 GB, reviewed the settings, and clicked Create.
Step 5: Creating and Formatting a Volume
Although the virtual disk had been created, it still needed to be formatted before it could store files.
35.	When prompted to create a volume, I left the option enabled and continued.
36.	I selected Lab Virtual Disk and clicked Next.
37.	I accepted the default volume size so that the entire virtual disk would be available for use.
38.	I assigned the drive letter Z: to make it easy to identify.
39.	For the file system, I selected NTFS and gave the volume the label Storage Pool. (Windows also offers ReFS, which provides additional resilience and modern storage features, but NTFS remains the most widely used file system and was sufficient for my lab.)
40.	After reviewing the configuration, I clicked Create to format the volume.
Once the wizard completed, I opened File Explorer and confirmed that the new Z: drive was available and ready to use.
Knowing that Windows was automatically distributing data across five separate virtual disks while maintaining parity protection gave me a much better understanding of how software-defined storage works. Even if one of the disks were to fail, the stored data could still be recovered because of the parity information spread across the remaining disks.
Verifying the Storage Configuration with PowerShell
To confirm that everything had been configured correctly, I opened PowerShell and ran the following commands:
Get-StoragePool
Get-VirtualDisk
Get-Volume
These commands allowed me to verify that the storage pool, virtual disk, and formatted volume had all been created successfully. Using PowerShell alongside the graphical management tools also gave me additional confidence that the Storage Spaces configuration was functioning exactly as intended.
Completing this exercise helped me understand how Windows Server can provide enterprise-style storage management without requiring dedicated RAID hardware. It also demonstrated how Storage Spaces can improve both flexibility and fault tolerance, making it a valuable feature for home labs as well as production environments.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/c4bbd8f8-1670-48cf-a53d-278ffbe5a582" />

Section 12: File Server. Creating Shared Folders and Mapping Network Drives
One of the most common responsibilities of a Windows Server is acting as a File Server. Whether it's documents, spreadsheets, project files, or departmental resources, organisations rely on file servers to provide a central location where employees can securely store, access, and share data.
In this section, I configured server01 to function as a file server by creating a shared folder on the Z: drive that I created using Storage Spaces. After configuring the appropriate permissions, I tested access from server02 and finished by mapping the shared folder as a network drive.
This exercise gave me a practical understanding of how shared folders are created and managed in a Windows Server environment.
Step 1: Verifying the File Server Role
Before creating any shared folders, I first confirmed that the File Server role was installed.
1.	I opened Server Manager, selected Manage, and clicked Add Roles and Features.
2.	I navigated to Server Roles → File and Storage Services → File and iSCSI Services.
3.	The File Server role was already selected, which is the default on most Windows Server installations. If it hadn't been installed, I would simply have selected it and completed the installation.
4.	After confirming the role was available, I closed the wizard and moved on to creating the shared folder.
Step 2: Creating the Shared Folder
With the File Server role available, I created the folder that would be shared across the network.
5.	I opened File Explorer and navigated to the Z: drive, which was the Storage Spaces volume I had created in the previous section.
6.	Inside the drive, I created a new folder named Projects.
This folder would serve as the shared location that users on other computers could access through the network.
Step 3: Sharing the Folder Using Server Manager
Although Windows allows folders to be shared by right-clicking them in File Explorer, I chose to use Server Manager instead. I found this approach more structured and closer to the way file shares are typically managed in professional environments, as it provides more configuration options and better visibility over existing shares.
7.	In Server Manager, I selected File and Storage Services, then clicked Shares.
8.	From the Tasks menu, I selected New Share.
9.	For the share type, I chose SMB Share – Quick, which is the standard option for Windows file sharing.
10.	Rather than selecting one of the suggested locations, I chose Type a custom path and browsed to:
Z:\Projects
After selecting the folder, I clicked Next.
11.	The share name automatically populated as Projects, which I left unchanged. I also added the description:
Projects folder – Staff access
before continuing.
12.	On the Other Settings page, I enabled Access-Based Enumeration.
I found this feature particularly useful because it hides folders from users who do not have permission to access them. Instead of seeing folders they cannot open, users only see the resources they are authorised to use, making the environment both cleaner and more secure.
13.	On the Permissions page, I selected Customize Permissions.
14.	In the NTFS permissions window, I added the Domain Users group and granted the appropriate permissions, including Read, Write, and List Folder Contents.
15.	After reviewing the configuration, I clicked Apply, then Next, and finally Create to publish the shared folder.
At this point, the Projects folder was available across the network.
Step 4: Testing the Shared Folder from Server02
Once the share had been created, I wanted to verify that another domain-joined server could access it successfully.
16.	I switched to server02 and opened File Explorer.
17.	In the address bar, I entered the following Universal Naming Convention (UNC) path:
\\server01\Projects
18.	The shared folder opened successfully. To confirm that I had the correct permissions, I created a small test file inside the folder.
19.	I then returned to server01 and opened the Z:\Projects folder. The file I had created on server02 appeared immediately, confirming that the share was working correctly and that both servers could access the same data.
Successfully completing this test gave me confidence that the file share had been configured correctly and that permissions were functioning as expected.
Step 5: Mapping the Shared Folder as a Network Drive
Although accessing a shared folder using its UNC path works perfectly, typing the full path every time can become inconvenient. To make access easier, I mapped the shared folder as a network drive.
20.	On server02, I opened File Explorer, right-clicked This PC, and selected Map Network Drive.
21.	I chose the drive letter Z:.
22.	In the Folder field, I entered:
\\server01\Projects
23.	I selected Reconnect at sign-in so that Windows would automatically recreate the drive mapping whenever I logged in.
24.	Finally, I clicked Finish.
The shared folder now appeared in File Explorer as the Z: drive on server02, making it feel just like a locally attached disk even though the data was stored on server01.
In a larger organisation, administrators wouldn't normally configure drive mappings manually for every user. Instead, this process would typically be automated using Group Policy, allowing network drives to be mapped automatically whenever users sign in. Seeing how simple the manual process was made it easier for me to appreciate how powerful Group Policy can be when managing hundreds or even thousands of computers.
By completing this section, I had successfully transformed server01 into a functioning file server. Users on other domain-joined machines could access shared resources over the network, store files centrally, and work with them as though they were located on their own computers. This is one of the core services provided by Windows Server in many enterprise environments, and implementing it in my lab gave me valuable hands-on experience with file sharing, permissions management, and network drive mapping.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/532dc367-f7f7-4e91-a82b-ac086445aee7" />

Section 13: Understanding and Configuring NTFS Permissions
After creating my shared folders, the next step was learning how to control who could access them and what they were allowed to do. This is where NTFS permissions come in.
I found NTFS permissions to be one of the most important concepts in Windows Server administration. At first, the different permission levels seemed confusing, but once I understood how they work together, it became much easier to see why they are used in almost every Windows environment.
Unlike share permissions, which control access over the network, NTFS permissions provide much more detailed control over files and folders stored on an NTFS-formatted drive. They allow administrators to decide exactly what actions users can perform, from simply viewing files to modifying or deleting them.
Understanding NTFS permissions is also a valuable skill from a career perspective, as they are commonly tested during technical interviews and used daily by IT Support Engineers and System Administrators.
Understanding the Different Permission Levels
Windows provides several standard NTFS permission levels, each granting a different degree of access.
Permission	What It Allows
Full Control	Gives complete control over files and folders, including reading, writing, modifying, deleting, and even changing permissions or taking ownership.
Modify	Allows users to read, edit, create, and delete files, but not change security permissions.
Read & Execute	Allows users to open files and run applications without making changes.
List Folder Contents	Allows users to view the contents of a folder without modifying them.
Read	Allows users to open and read files but prevents any modifications.
Write	Allows users to create new files and edit existing ones, but not delete them.
Learning the differences between these permission levels helped me understand how organisations protect sensitive information while still allowing employees to perform their jobs.
Understanding Permission Inheritance
Another important concept I learned was inheritance.
By default, every file and folder inside Windows inherits its permissions from its parent folder. This saves administrators from having to configure permissions individually for every single file or subfolder.
For example, if the Projects folder grants the Domain Users group Read access, every folder created inside Projects automatically receives the same permissions unless inheritance is changed.
This behaviour makes administration much simpler because permissions remain consistent throughout the folder structure.
However, there are situations where a specific folder needs tighter security than its parent. In those cases, inheritance can be disabled so that custom permissions can be applied.
Restricting Access to a Specific Folder
To see how this worked in practice, I created a folder that only members of the Air Staff security group would be able to access.
1.	Inside the Projects folder, I created a new subfolder named Air Staff Only.
2.	I right-clicked the folder, selected Properties, opened the Security tab, and clicked Advanced.
3.	I selected Disable inheritance.
4.	Windows asked how I wanted to handle the existing permissions. Rather than removing everything, I chose Convert inherited permissions into explicit permissions. This copied the existing permissions into the folder, giving me a safe starting point without having to recreate every permission manually.
5.	I removed the Domain Users group from the permissions list.
6.	Next, I clicked Add, selected the Air Staff security group, and granted it Modify permissions that applied to This folder, subfolders, and files.
7.	After reviewing the settings, I clicked Apply and then OK to save the changes.
Once the permissions were applied, only members of the Air Staff security group could access the folder.
Because I had previously enabled Access-Based Enumeration when creating the network share, users who were not members of the Air Staff group couldn't even see that the Air Staff Only folder existed. From their perspective, it simply didn't appear inside the shared folder, providing an additional layer of security and reducing confusion.
A Best Practice I Learned
One of the most valuable lessons I took away from this exercise was a best practice followed in most professional environments:
Never assign permissions directly to individual user accounts whenever it can be avoided. Instead, assign permissions to security groups.
Using security groups makes administration far more efficient. If an employee joins a department, I only need to add them to the appropriate security group. If they leave the company or move to another role, I simply remove them from that group. The folder permissions never need to be changed because access is managed through group membership rather than individual accounts.
This approach not only reduces administrative effort but also helps maintain consistency, improves security, and makes troubleshooting permissions much easier.
By completing this section, I gained a much clearer understanding of how Windows controls access to files and folders. Combined with Active Directory security groups and shared folders, NTFS permissions provide a powerful and flexible way to ensure that users can access only the resources they need while protecting sensitive information from unauthorized access.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/286ed57c-0e65-4fa4-b795-0c9aef061c67" />

Section 14: Managing Windows Updates
After configuring the core services in my Windows Server environment, one of the final administrative tasks I explored was Windows Update. While installing roles and configuring Active Directory are important, keeping a server updated is just as critical.
Microsoft regularly releases updates to address newly discovered security vulnerabilities, improve system stability, fix bugs, and occasionally introduce new features. One thing I learned during this project is that an unpatched server can quickly become a security risk. In fact, many successful cyberattacks exploit systems that are missing important security updates.
Because of this, regularly checking for and applying Windows Updates is an essential part of maintaining a healthy Windows Server environment.
Checking for Updates Using the Graphical Interface
The simplest way to manage updates is through the Windows Settings application.
1.	I opened the Start menu and searched for Settings.
2.	From the Settings window, I selected Windows Update, located near the bottom of the navigation pane.
3.	I clicked Check for updates. Windows contacted Microsoft's update servers and searched for any available security patches, cumulative updates, or feature updates.
4.	If updates were available, I selected Download and install. Once the installation completed, Windows prompted me to restart the server to finish applying the updates.
Keeping the operating system fully updated helps ensure that the server remains secure, stable, and compatible with the latest Microsoft improvements.
Exploring Useful Windows Update Settings
While reviewing the Windows Update section, I also explored several settings that administrators commonly use to control how updates are installed.
Pause Updates
This option allows updates to be temporarily postponed for a limited period. I learned that this can be useful during critical business periods—for example, when an organisation is processing payroll or carrying out an important system migration and wants to avoid unexpected restarts.
Update History
The Update History page provides a record of every update that has been installed on the server. It shows when each update was installed and whether the installation completed successfully or encountered any issues.
I found this particularly useful because it provides a quick way to verify whether a recent update has been applied or to investigate update-related problems.
Active Hours
The Active Hours feature allows administrators to define the times during which the server is normally in use.
By configuring Active Hours, Windows avoids automatically restarting the server during those periods, reducing the likelihood of disrupting users or important services.
Receiving Updates for Other Microsoft Products
Within Advanced Options, I enabled Receive updates for other Microsoft products.
This setting allows Windows Update to install updates for Microsoft applications such as Microsoft Office alongside Windows updates, helping ensure that the entire Microsoft software environment remains secure and up to date.
Managing Updates with SConfig
Although the graphical interface works well, I also learned that Windows Server includes a command-line utility called SConfig, which provides a faster way to manage updates—particularly when administering multiple servers.
To launch the tool, I opened Windows Terminal (Administrator) and entered:
sconfig
From the SConfig menu, I selected Option 5 – Windows Update Settings.
This menu allows administrators to choose how Windows should handle updates:
•	Automatic (A): Windows automatically downloads and installs updates before restarting the server outside configured Active Hours.
•	Download Only (D): Updates are downloaded automatically, but installation only occurs after administrator approval.
•	Manual (M): The administrator controls when updates are downloaded, installed, and when the server is restarted.
For a home lab, any of these options can work depending on personal preference. However, I found that Manual offers the greatest level of control because it allows updates to be installed at a convenient time without unexpected reboots interrupting ongoing work. This is also a common approach for production servers, where administrators carefully plan maintenance windows before applying updates.
Windows Updates in Enterprise Environments
While working through this exercise, I also discovered that large organisations rarely manage Windows Updates on each server individually.
Instead, updates are usually deployed centrally using enterprise management solutions such as:
•	Windows Server Update Services (WSUS), which allows organisations to approve and distribute Microsoft updates from an internal server.
•	Microsoft Endpoint Configuration Manager (MECM/SCCM), which provides advanced software deployment, patch management, and device administration across large environments.
•	Remote Monitoring and Management (RMM) platforms, which enable managed service providers and IT teams to monitor systems and deploy updates across multiple client networks from a single management console.
Using these centralised tools allows administrators to test updates before deployment, schedule maintenance windows, monitor compliance, and ensure that every server and workstation receives the required security patches without having to manage each machine individually.
Completing this section reinforced an important lesson: installing Windows Server is only the beginning. Keeping the server patched and regularly maintained is an ongoing responsibility that plays a vital role in protecting systems from security threats, improving reliability, and ensuring the overall health of the IT environment.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7f8942ad-c390-4373-a60c-e68bfc9ce408" />

Section 15: Using Event Viewer to Read and Filter Logs
As I progressed through my Windows Server project, I wanted to become familiar with one of the most important troubleshooting tools available to every Windows administrator—Event Viewer.
Almost everything that happens on a Windows Server is recorded somewhere, and Event Viewer is where those records are stored. Whether a user signs in, a service starts successfully, a driver fails, a piece of hardware encounters an error, or a security event occurs, Windows logs the activity for future reference.
I quickly realised that knowing how to navigate Event Viewer is an essential skill for any IT Support Engineer or System Administrator. Rather than guessing why something isn't working, Event Viewer often provides the evidence needed to identify the root cause of a problem.
Opening Event Viewer
To access Event Viewer, I simply:
1.	Right-clicked the Start menu and selected Event Viewer.
Alternatively, I could open the Start menu, search for Event Viewer, and launch it from the search results.
Once the console opened, I could immediately begin exploring the different categories of logs that Windows maintains.
Understanding the Windows Log Categories
In the left-hand navigation pane, I expanded Windows Logs, where I found several different log categories. Each one records a different type of activity within the operating system.
Application
The Application log contains events generated by software installed on the server. This includes Microsoft applications, third-party software, databases, and custom business applications.
If an application crashes or encounters an unexpected error, this is usually one of the first places I would investigate.
Security
The Security log records authentication and auditing events.
Examples include:
•	Successful and failed sign-in attempts
•	Account lockouts
•	User account creation and deletion
•	Changes to security groups
•	Permission changes
•	Group Policy events
I found this log particularly important because it plays a major role in security monitoring, auditing, and incident investigations.
Setup
The Setup log records events related to Windows installation, server roles, Windows updates, and component installations.
This log can be especially useful when troubleshooting installation failures or deployment issues.
System
The System log contains events generated by Windows itself.
Typical events include:
•	Driver failures
•	Hardware issues
•	Service startup failures
•	System shutdown events
•	Operating system errors
Whenever I need to investigate why Windows isn't behaving as expected, the System log is usually one of the first places I check.
Forwarded Events
The Forwarded Events log stores events that have been collected from other Windows computers and forwarded to this server.
Although I didn't configure event forwarding in my lab, I learned that this feature is commonly used in enterprise environments to centralise log collection from multiple servers.
Understanding Event Levels
Every event recorded in Event Viewer is assigned a severity level, making it easier to identify which entries deserve immediate attention.
Information
These are normal operational messages indicating that Windows or an application has completed an expected action successfully.
Most events recorded on a healthy server fall into this category.
Warning
Warnings indicate that something unusual has occurred.
The issue may not have caused an immediate failure, but it could become a problem if left unresolved.
Error
Errors indicate that something has gone wrong.
Examples include failed services, application crashes, hardware problems, or driver failures.
These are often the events I would investigate first when troubleshooting a problem.
Critical
Critical events represent the most serious issues.
They usually indicate that the operating system or a major component experienced a severe failure that affected the stability or availability of the server.
Reading an Event
To better understand how Event Viewer works, I selected several events and reviewed the information displayed in the details pane.
The fields I found most useful were:
•	Event ID – A unique numerical identifier assigned to each type of event.
I discovered that searching for an Event ID online often leads directly to Microsoft documentation or community discussions explaining the event and possible solutions.
•	Source – Identifies the application, service, or Windows component that generated the event.
•	Description – Provides a detailed explanation of what occurred, often including enough information to begin troubleshooting.
Learning to interpret these three fields made Event Viewer much less intimidating and significantly easier to navigate.
Filtering Event Logs
One thing I noticed immediately was how quickly Event Viewer fills with information. Even a freshly installed server can generate hundreds or thousands of events within a short period.
Rather than scrolling through everything manually, I learned how to filter the logs to display only the events that were relevant to my investigation.
1.	I right-clicked a log, such as System, and selected Filter Current Log.
2.	Under Event Level, I selected Critical and Error to display only the most serious issues.
3.	If I wanted to investigate a specific event, I could also filter by Event ID. For example, entering 4624 or 4625 in the Security log would display only those events.
4.	After clicking OK, Event Viewer displayed only the matching log entries, making troubleshooting much faster.
Security Event IDs I Learned
While exploring the Security log, I came across several Event IDs that administrators frequently monitor during security investigations.
Event ID	Description
4624	Successful user logon
4625	Failed logon attempt (often useful when investigating possible brute-force attacks)
4740	User account locked out
4720	User account created
4726	User account deleted
4728	User added to a security-enabled global group
These Event IDs are among the most commonly referenced during Windows security monitoring, and becoming familiar with them gave me a better understanding of what administrators look for when reviewing security logs.
Creating a Custom View
To make future troubleshooting easier, I created a custom view that automatically displays the most important events from every Windows log.
5.	In the navigation pane, I right-clicked Custom Views and selected Create Custom View.
6.	I selected Critical and Error as the event levels.
7.	Under By Log, I chose All Windows Logs.
8.	Finally, I clicked OK and named the view All Errors and Criticals.
Now, whenever I open Event Viewer, I can simply select this custom view to see every critical error across the entire server without having to search through each log individually.
Creating this custom view made troubleshooting much more efficient and showed me how Windows administrators can tailor Event Viewer to suit their own workflow.
By the end of this section, I had developed a much better understanding of how Windows Server records system activity and how those logs can be used to investigate problems. Rather than relying on guesswork, I now know how to locate important events, filter out unnecessary information, identify meaningful Event IDs, and create customised views that highlight the issues requiring immediate attention. These are practical skills that I expect to use regularly in both IT support and system administration roles.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/c5907821-ebbb-48c5-914e-ce2ddc7b7f9e" />

Section 16: Windows Defender Firewall – Creating and Managing Firewall Rules
As I continued building my Windows Server lab, I wanted to understand how Windows controls network traffic entering and leaving the server. This is where Windows Defender Firewall with Advanced Security plays a vital role.
Every connection made to or from a Windows Server is evaluated by the firewall before it is allowed to proceed. Based on a set of predefined rules, the firewall decides whether the traffic should be permitted or blocked.
One thing that became clear while working through this section is that a properly configured firewall is essential for server security. If the firewall is too restrictive, legitimate users and services may be unable to communicate with the server. On the other hand, allowing unnecessary traffic can expose the server to potential security threats. The goal is to strike the right balance by allowing only the connections that are genuinely required.
Opening Windows Defender Firewall with Advanced Security
To begin exploring the firewall, I opened the advanced management console.
1.	I opened the Start menu and searched for Windows Defender Firewall with Advanced Security.
2.	I launched the application, which provides much more detailed control than the standard Windows Firewall interface.
From here, I could view existing firewall rules, create new ones, modify existing rules, and monitor how the firewall was protecting the server.
Understanding Inbound and Outbound Rules
As I explored the console, I noticed that firewall rules are divided into two main categories: Inbound Rules and Outbound Rules.
Inbound Rules
Inbound rules determine what network traffic is allowed to reach the server.
Whenever another computer attempts to connect to my server—whether through Remote Desktop, DNS, DHCP, or a web application—the firewall checks the inbound rules to decide whether that connection should be permitted.
These are generally the rules administrators spend the most time managing because they directly control who and what can access server services.
Outbound Rules
Outbound rules control the traffic leaving the server.
By default, Windows allows most outbound traffic, which is sufficient for many environments. However, in highly secure organisations, outbound rules are often used to restrict which applications or services are allowed to communicate with external systems, helping to reduce the risk of malware or unauthorised data transfers.
Reviewing the Existing Firewall Rules
Before creating any new rules, I spent some time reviewing the firewall configuration that Windows had already created automatically.
I selected Inbound Rules and found that numerous rules were already present.
This immediately made sense because earlier in the project I had installed several server roles, including:
•	Active Directory Domain Services (AD DS)
•	DNS Server
•	DHCP Server
During installation, Windows automatically created the firewall rules required for each of these services to function correctly.
For example:
•	DHCP Server automatically creates rules allowing communication over UDP ports 67 and 68.
•	DNS Server creates rules that allow both TCP and UDP traffic on port 53.
•	Active Directory Domain Services creates a larger collection of rules to support authentication, directory services, replication, and communication between domain controllers and client computers.
I found this particularly useful because it demonstrated how Windows Server simplifies administration by configuring many essential firewall settings automatically. Even though I didn't need to create these rules myself, reviewing them helped me understand which services required which network ports.
Creating a Custom Inbound Rule
To gain hands-on experience with firewall management, I created my own inbound rule.
For this example, I imagined that I had deployed a web application running on TCP port 8080. Since Windows Firewall blocks unsolicited traffic by default, I needed to create a rule that would allow incoming connections on that port.
3.	In Windows Defender Firewall with Advanced Security, I right-clicked Inbound Rules and selected New Rule.
4.	I chose Port as the rule type and clicked Next.
5.	I selected TCP, then entered 8080 under Specific local ports before continuing.
6.	I selected Allow the connection.
7.	I applied the rule to all available network profiles (Domain, Private, and Public) and clicked Next.
8.	Finally, I named the rule:
Allow TCP 8080 – Web App
and clicked Finish.
The rule became active immediately, allowing devices on the network to connect to the server using TCP port 8080, provided that a service was actively listening on that port.
A Key Security Principle I Learned
While creating firewall rules, I was reminded of an important cybersecurity principle:
Only open the ports that are absolutely necessary.
Every open network port represents another potential entry point into a system. If a service is no longer required, its associated firewall rule should be reviewed and, where appropriate, disabled or removed.
This follows the Principle of Least Privilege, which states that systems should only be given the minimum level of access required to perform their intended function. The same philosophy applies to firewall configuration deny everything by default and explicitly allow only the traffic that is genuinely needed.
By completing this section, I gained practical experience with one of the most important security features built into Windows Server. I learned how firewall rules regulate network communication, how Windows automatically creates rules for installed server roles, and how to manually create custom rules for new applications. More importantly, I developed a better appreciation of how proper firewall management helps protect servers while still allowing legitimate services to remain accessible.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ab464da3-6de0-462f-93a6-a6ed6f2de07a" />

Section 17: Microsoft Defender Antivirus
After completing the core configuration of my Windows Server lab, I wanted to make sure the server was protected against malware and other security threats. Thankfully, Windows Server 2025 comes with Microsoft Defender Antivirus built in, so I didn't need to install any additional antivirus software before securing the system.
While many organisations complement Defender with dedicated Endpoint Detection and Response (EDR) platforms, I found that Microsoft Defender Antivirus provides a solid baseline level of protection. For a home lab like mine—or any server that doesn't yet have a third-party security solution installed—it offers more than enough functionality to help keep the system secure.
Working through this section gave me practical experience with verifying that Defender was running correctly, performing antivirus scans, updating security intelligence, and exploring the settings that help protect a Windows Server from modern threats.
Verifying Microsoft Defender Was Running
Before doing anything else, I wanted to confirm that Microsoft Defender was active.
1.	I opened the Start menu and searched for Windows Security.
2.	On the Security at a glance page, I checked that Virus & threat protection displayed a green tick, confirming that Microsoft Defender Antivirus was active and protecting the server.
If Defender had displayed a warning or indicated that protection was turned off, I would have opened the section and enabled it before continuing.
3.	I also confirmed that App & browser control was enabled, providing another layer of protection against malicious applications and unsafe downloads.
Seeing every protection area marked as healthy gave me confidence that the server's built-in security features were functioning as expected.

Running a Malware Scan
With Defender confirmed to be running, I performed a malware scan to verify that the server was free from any known threats.
4.	From Windows Security, I opened Virus & threat protection.
5.	I selected Quick Scan.
A Quick Scan focuses on the parts of Windows where malware is most likely to hide, making it a fast and efficient way to check the overall health of the system. On my server, the scan completed in just a few minutes.
6.	I also explored Scan options, where I found the Full Scan feature.
Unlike a Quick Scan, a Full Scan examines every file on every drive attached to the server. Although it takes significantly longer to complete, it provides a much more comprehensive inspection and is ideal for routine maintenance or whenever suspicious behaviour needs to be investigated.
Understanding the difference between these scan types helped me appreciate when each one would be most appropriate.

Updating Security Intelligence
One thing I quickly realised is that antivirus software is only effective if it can recognise the latest threats.
Microsoft Defender relies on regularly updated security intelligence, sometimes referred to as virus definitions, to identify newly discovered malware.
To make sure my server was using the latest protection, I completed the following steps:
7.	Within Virus & threat protection, I scrolled down to Virus & threat protection updates.
8.	I selected Check for updates.
If newer security intelligence was available, Windows automatically downloaded and installed it.
This reminded me that keeping antivirus definitions up to date is just as important as keeping Windows itself updated. As new malware variants appear every day, regularly updating Defender ensures it remains capable of detecting the latest threats.

Reviewing Microsoft Defender Settings
To better understand how Microsoft Defender protects Windows Server in real time, I opened:
Virus & threat protection → Manage settings
Here, I reviewed the key security features available.
Real-time Protection
The first setting I checked was Real-time Protection, which was enabled.
This feature continuously monitors files as they are created, opened, downloaded, copied, or modified. If Defender detects malicious activity, it can stop the threat before it has an opportunity to execute.
Since this provides continuous protection, I would always leave it enabled unless I had a very specific troubleshooting reason not to.
Cloud-delivered Protection
Next, I reviewed Cloud-delivered Protection.
Rather than relying only on locally stored malware definitions, this feature allows Defender to consult Microsoft's cloud-based threat intelligence whenever suspicious files are detected.
This enables Microsoft Defender to identify and respond to newly emerging threats much faster than relying solely on traditional signature updates.
Automatic Sample Submission
I also looked at Automatic Sample Submission.
When enabled, Microsoft Defender can securely send suspicious files to Microsoft for further analysis. This helps Microsoft improve malware detection and strengthen protection across Windows devices worldwide.
Although enabling this setting is optional, I can see how it contributes to faster identification of new threats.
Tamper Protection
One of the features I found most reassuring was Tamper Protection.
This setting prevents malware—or even unauthorised users—from disabling Microsoft Defender or modifying important security settings.
Since many forms of malware attempt to switch off antivirus protection before launching an attack, keeping Tamper Protection enabled adds another valuable layer of defence.
Exclusions
Finally, I reviewed the Exclusions section.
Exclusions allow trusted files, folders, or applications to be ignored during antivirus scanning.
While this can solve issues where legitimate software is mistakenly identified as malicious, I learned that exclusions should only be used after carefully verifying that the application is completely safe. Creating unnecessary exclusions could leave parts of the system unprotected.

Microsoft Defender in Enterprise Environments
As I researched Microsoft Defender further, I discovered that many organisations build upon its capabilities with more advanced endpoint security platforms.
One example is Microsoft Defender for Endpoint, which extends the standard antivirus features by providing:
•	Centralised security management
•	Advanced behavioural threat detection
•	Threat intelligence
•	Automated investigation and response
•	Endpoint visibility across an entire organisation
I also learned that many businesses use third-party Endpoint Detection and Response (EDR) solutions such as CrowdStrike Falcon, SentinelOne, or Sophos Intercept X, depending on their operational requirements and existing security infrastructure.
These platforms provide deeper visibility into endpoint activity and enable security teams to detect, investigate, and respond to sophisticated cyber threats across thousands of devices from a central management console.

Reflection
Completing this section gave me a much stronger understanding of Microsoft's built-in endpoint protection and how it fits into the overall security of a Windows Server environment. Rather than simply relying on the default settings, I learned how to verify that Defender was operating correctly, perform both quick and full malware scans, update its security intelligence, and review the features responsible for protecting the server in real time.
More importantly, this exercise reinforced that effective server security isn't achieved through a single tool. It comes from combining multiple layers of protection—including regular Windows updates, a properly configured firewall, strong access controls, and continuously updated antivirus software. Microsoft Defender Antivirus forms an important part of that layered security approach, and gaining hands-on experience with it has strengthened both my understanding of Windows Server administration and my appreciation for proactive endpoint security.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/8214f147-88c8-4fb7-8121-c3141fcf4b83" />

Section 17: Microsoft Defender Antivirus
After completing the core configuration of my Windows Server lab, I wanted to make sure the server was protected against malware and other security threats. Thankfully, Windows Server 2025 comes with Microsoft Defender Antivirus built in, so I didn't need to install any additional antivirus software before securing the system.
While many organisations complement Defender with dedicated Endpoint Detection and Response (EDR) platforms, I found that Microsoft Defender Antivirus provides a solid baseline level of protection. For a home lab like mine—or any server that doesn't yet have a third-party security solution installed—it offers more than enough functionality to help keep the system secure.
Working through this section gave me practical experience with verifying that Defender was running correctly, performing antivirus scans, updating security intelligence, and exploring the settings that help protect a Windows Server from modern threats.
Verifying Microsoft Defender Was Running
Before doing anything else, I wanted to confirm that Microsoft Defender was active.
1.	I opened the Start menu and searched for Windows Security.
2.	On the Security at a glance page, I checked that Virus & threat protection displayed a green tick, confirming that Microsoft Defender Antivirus was active and protecting the server.
If Defender had displayed a warning or indicated that protection was turned off, I would have opened the section and enabled it before continuing.
3.	I also confirmed that App & browser control was enabled, providing another layer of protection against malicious applications and unsafe downloads.
Seeing every protection area marked as healthy gave me confidence that the server's built-in security features were functioning as expected.

Running a Malware Scan
With Defender confirmed to be running, I performed a malware scan to verify that the server was free from any known threats.
4.	From Windows Security, I opened Virus & threat protection.
5.	I selected Quick Scan.
A Quick Scan focuses on the parts of Windows where malware is most likely to hide, making it a fast and efficient way to check the overall health of the system. On my server, the scan completed in just a few minutes.
6.	I also explored Scan options, where I found the Full Scan feature.
Unlike a Quick Scan, a Full Scan examines every file on every drive attached to the server. Although it takes significantly longer to complete, it provides a much more comprehensive inspection and is ideal for routine maintenance or whenever suspicious behaviour needs to be investigated.
Understanding the difference between these scan types helped me appreciate when each one would be most appropriate.

Updating Security Intelligence
One thing I quickly realised is that antivirus software is only effective if it can recognise the latest threats.
Microsoft Defender relies on regularly updated security intelligence, sometimes referred to as virus definitions, to identify newly discovered malware.
To make sure my server was using the latest protection, I completed the following steps:
7.	Within Virus & threat protection, I scrolled down to Virus & threat protection updates.
8.	I selected Check for updates.
If newer security intelligence was available, Windows automatically downloaded and installed it.
This reminded me that keeping antivirus definitions up to date is just as important as keeping Windows itself updated. As new malware variants appear every day, regularly updating Defender ensures it remains capable of detecting the latest threats.

Reviewing Microsoft Defender Settings
To better understand how Microsoft Defender protects Windows Server in real time, I opened:
Virus & threat protection → Manage settings
Here, I reviewed the key security features available.
Real-time Protection
The first setting I checked was Real-time Protection, which was enabled.
This feature continuously monitors files as they are created, opened, downloaded, copied, or modified. If Defender detects malicious activity, it can stop the threat before it has an opportunity to execute.
Since this provides continuous protection, I would always leave it enabled unless I had a very specific troubleshooting reason not to.
Cloud-delivered Protection
Next, I reviewed Cloud-delivered Protection.
Rather than relying only on locally stored malware definitions, this feature allows Defender to consult Microsoft's cloud-based threat intelligence whenever suspicious files are detected.
This enables Microsoft Defender to identify and respond to newly emerging threats much faster than relying solely on traditional signature updates.
Automatic Sample Submission
I also looked at Automatic Sample Submission.
When enabled, Microsoft Defender can securely send suspicious files to Microsoft for further analysis. This helps Microsoft improve malware detection and strengthen protection across Windows devices worldwide.
Although enabling this setting is optional, I can see how it contributes to faster identification of new threats.
Tamper Protection
One of the features I found most reassuring was Tamper Protection.
This setting prevents malware—or even unauthorised users—from disabling Microsoft Defender or modifying important security settings.
Since many forms of malware attempt to switch off antivirus protection before launching an attack, keeping Tamper Protection enabled adds another valuable layer of defence.
Exclusions
Finally, I reviewed the Exclusions section.
Exclusions allow trusted files, folders, or applications to be ignored during antivirus scanning.
While this can solve issues where legitimate software is mistakenly identified as malicious, I learned that exclusions should only be used after carefully verifying that the application is completely safe. Creating unnecessary exclusions could leave parts of the system unprotected.

Microsoft Defender in Enterprise Environments
As I researched Microsoft Defender further, I discovered that many organisations build upon its capabilities with more advanced endpoint security platforms.
One example is Microsoft Defender for Endpoint, which extends the standard antivirus features by providing:
•	Centralised security management
•	Advanced behavioural threat detection
•	Threat intelligence
•	Automated investigation and response
•	Endpoint visibility across an entire organisation
I also learned that many businesses use third-party Endpoint Detection and Response (EDR) solutions such as CrowdStrike Falcon, SentinelOne, or Sophos Intercept X, depending on their operational requirements and existing security infrastructure.
These platforms provide deeper visibility into endpoint activity and enable security teams to detect, investigate, and respond to sophisticated cyber threats across thousands of devices from a central management console.
Reflection
Completing this section gave me a much stronger understanding of Microsoft's built-in endpoint protection and how it fits into the overall security of a Windows Server environment. Rather than simply relying on the default settings, I learned how to verify that Defender was operating correctly, perform both quick and full malware scans, update its security intelligence, and review the features responsible for protecting the server in real time.
More importantly, this exercise reinforced that effective server security isn't achieved through a single tool. It comes from combining multiple layers of protection—including regular Windows updates, a properly configured firewall, strong access controls, and continuously updated antivirus software. Microsoft Defender Antivirus forms an important part of that layered security approach, and gaining hands-on experience with it has strengthened both my understanding of Windows Server administration and my appreciation for proactive endpoint security.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/4e52c963-f92c-43ed-a4d8-b6547e41f810" />

Summary: What I Built and What I Learned
When I started this home lab project, all I had was a fresh installation of VMware Workstation, a Windows Server 2025 ISO, and a willingness to learn. At the time, I understood the theory behind many Windows Server concepts, but I had very little hands-on experience putting them into practice. Looking back now, it's rewarding to see how much I've accomplished.
By the end of this project, I had built a fully functional Windows Server 2025 lab environment that closely mirrors the type of infrastructure used in many real-world organisations. More importantly, I didn't just follow a set of instructions—I took the time to understand why each technology exists, how it works, and how it connects with everything else in the environment.
Over the course of this project, I successfully built and configured:
1.	A Windows Server 2025 virtual machine running in VMware Workstation on my Windows 11 computer.
2.	A fully configured Domain Controller (server01) running Active Directory Domain Services (AD DS) and DNS.
3.	A functioning DNS infrastructure, including manually created A, CNAME, and PTR records for internal name resolution.
4.	A DHCP Server capable of automatically assigning IP addresses and network settings to devices on the network.
5.	A structured Active Directory environment containing Organisational Units (OUs), user accounts, and security groups that reflect how identities are managed in a business environment.
6.	A second Windows Server (server02) successfully joined to the domain and managed through Active Directory.
7.	Group Policy Objects (GPOs) to centrally manage user settings, including restricting access to the Control Panel for standard users.
8.	A Storage Spaces configuration using five virtual hard disks combined into a parity storage pool, presented as my Z: drive.
9.	A File Server with shared folders, properly configured NTFS permissions, security group-based access control, and mapped network drives.
10.	Custom and built-in Windows Defender Firewall rules to manage inbound network traffic securely.
11.	Microsoft Defender Antivirus, including malware scanning, security intelligence updates, and configuration of key protection features.
12.	Windows Server Backup, configured to perform scheduled backups and successfully tested through a file recovery exercise.
13.	Event Viewer, including custom log filtering and a dedicated view for monitoring critical errors and system events.
Every one of these tasks represents a practical skill that Windows Server administrators use on a regular basis. Technologies such as Active Directory, DNS, DHCP, Group Policy, NTFS permissions, Windows Firewall, Microsoft Defender, and Windows Server Backup form the foundation of many enterprise IT environments.
Beyond learning how to configure these services, I also developed a much better understanding of how they work together. For example, I saw firsthand how Active Directory depends on DNS, how DHCP distributes network settings that point clients to the domain controller, how Group Policy relies on Active Directory's organisational structure, and how NTFS permissions work alongside security groups to protect shared resources. Seeing these technologies interact made Windows Server feel much less like a collection of individual features and much more like an integrated ecosystem.
Perhaps the biggest lesson I learned throughout this project is that practical experience is irreplaceable. Reading documentation and watching tutorials helped me understand the concepts, but building everything myself, troubleshooting mistakes, and fixing problems gave me a level of confidence that theory alone never could.
This lab has also given me a strong foundation for future learning. Whether I continue with certifications such as CompTIA Server+, Microsoft AZ-800: Administering Windows Server Hybrid Core Infrastructure, Microsoft AZ-801, or vendor-neutral cybersecurity certifications, the skills I've developed here will continue to be relevant because they represent the core technologies used throughout enterprise Windows environments.




















  


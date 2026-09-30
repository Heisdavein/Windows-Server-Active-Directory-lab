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
________________________________________
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
________________________________________
Creating a Snapshot
Before powering on the virtual machine for the first time, I created a VMware snapshot.
Using VM → Snapshot → Take Snapshot, I saved the current state of the virtual machine and named it Before Install.
This snapshot provided a clean recovery point that I could return to at any time if the installation failed or if I wanted to restart the project without creating a brand-new virtual machine from scratch.
Although creating snapshots is optional, I found them incredibly useful throughout the project. They allowed me to experiment confidently, knowing I could always roll the virtual machine back to a known working state within minutes.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/3bb97711-3d19-4bb4-b225-b0f7285637bc" />

  


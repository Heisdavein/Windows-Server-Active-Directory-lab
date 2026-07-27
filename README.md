# Windows Server 2025 Home Lab

This is my personal project where I built a mini version of a company computer network, all inside my own PC, using free software. I set up a server, connected another computer to it, and configured a bunch of things that real IT professionals use every day at their jobs.

**Author:** Clinton Kehinde
The complete step-by-step build, including screenshots and detailed explanations, is available here:

**➡️ [Windows Server 2025 Home Lab Documentation (PDF)](https://github.com/Heisdavein/Windows-Server-Active-Directory-lab/tree/main)

---

## What is this project?

Big companies use something called a "server" — basically a powerful computer that manages everyone else's computers, logins, passwords, shared files, and internet settings. I wanted to learn how that works, so I built my own server at home using a program called VMware, which lets you run a "fake" computer (called a virtual machine) inside your real computer, without messing anything up.

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
- Set up a **file server** — a shared folder that other computers on the network could open, use, and save files to
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
- ## 📄 Full Documentation

The complete step-by-step build, including screenshots and detailed explanations, is available here:

**➡️ [Windows Server 2025 Home Lab Documentation (PDF)](https://github.com/Heisdavein/Windows-Server-Active-Directory-lab/tree/main)

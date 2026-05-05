---
title: Home Server
excerpt: Researching, assembling & configuring a home-server to self-host useful services using Docker
---

A home-server appealed to me for multiple reasons;
 * Not beholden to service providers - Many online services can be subject to undesirable changes of terms or pricing, prioritising profit over user-experience
 * Learning opportunity - Configuring a server filled a gap in my knowledge, particularly regarding networking & containerisation
 * Digital sovereignty - I believe in the principle of people being responsible for their own data when reasonably viable

So far, my home server can do the following;
 * Use ZFS pools for data redundancy across multiple drives for integrity
 * SMB shares to access data from other devices within the network
 * Streaming media such as films, music & images
 * CalDAV/CardDAV server, for storing contacts, events & tasks with network access
 * Git repository, for storing repos within my network rather than an online service
 * Bookmarks storage, making using multiple internet browsers easier without losing bookmarks
 * Syncing folders, which I use for having markdown notes being accessible & editable from multiple devices

I had also decided to purchase a mini PC with an Intel N100 to use as a custom router & firewall using OPNsense, to expand the configuration options compared to an ISP-provided device and be able to keep settings upon moving address or provider. Within this device I included a VPN instance using WireGuard, which allows me to connect to this network from other devices remotely, such as my phone or laptop connected to a different network. My network is set up like so;

![Home network diagram](/assets/images/projects/homeserver/diagram.png)
 
In the future I would like to implement;
 * Off-site backups, following the 3-2-1 rule to account for this site being compromised (Fire, burglery, etc)
 * Password manager, to make using secure passwords across multiple services easier
 * Reverse Proxy, allowing me to access services via my purchased domain-name rather than by IP address
 * Home automation, for down the line I would like to implement certain devices to be controlled via phone or desktop

 Although I'd had experience in researching computer hardware specifications for a personal computer that can run modern games, server hardware has different criteria. Power efficiency, particularly at lower draw, is important as servers spend much of their time idling waiting for incoming tasks, which influenced my choice of PSU & CPU. Understanding my processing requirements took knowing what I would like to use it for - Since I don't (for now) desire running local AI models, or simultaneous HD video encoding, a dedicated graphics card wasn't necessary and I settled for an integrated one within the CPU. 
 Memory is another aspect which has some changes, as for extra data-integrity, ECC (Error-correcting code) is useful for dealing with single-bit data corruption. This goes in-hand with the extra data stability provided by using ZFS pools in RAIDz or Mirror'd setups. This takes checking that the motherboard, CPU & RAM are all ECC compatible. 

I made the decision to stick with free, open-source solutions when possible, as it aligns with the aforementioned appeals of a home server, that being avoiding for-profit versus for-experience changes, and digital sovereignty. Should any changes be made which compromises user experience, there will almost certainly be a fork made which excludes these changes. 
I decided to use the TrueNAS operating system on my server, it provides much of my requirements for data storage & access with ZFS pools & SMB shares, whilst providing an option to run containerised services. I installed Komodo as an application, and used compose files to install other services through Komodo, as it provided more customisability over TrueNAS's in-built app service. 
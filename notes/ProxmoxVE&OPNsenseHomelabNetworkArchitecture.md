# Proxmox VE & OPNsense Homelab Network Architecture

I'm running **Proxmox Virtual Environment**, a Type 1 virtualization platform, directly on my Dell OptiPlex server.

Proxmox allows me to run multiple virtual machines, each with its own virtual Network Interface Cards (NICs). These virtual NICs connect to **Linux bridges**, which function as virtual Ethernet switches and allow virtual machines to communicate with one another and with physical networks.

For my network architecture, I've configured **two Linux bridges**.

**The first bridge, `vmbr0`, serves as my upstream network switch.** It's connected to the Dell's physical Ethernet interface `nic0`, which will connect to the household router. This bridge provides network connectivity to the Proxmox management interface and the Wide Area Network (WAN) interface of my OPNsense virtual firewall.

**The second bridge, `vmbr1`, serves as my internal Local Area Network (LAN) switch.** It's connected to another physical Ethernet interface, `nic2`, which connects to my wireless access point through a Power over Ethernet (PoE) injector.

OPNsense has **two virtual NICs**: one connected to `vmbr0` for upstream communication and another connected to `vmbr1` for the internal lab network.

OPNsense functions as the **router and firewall** between these two networks, controlling traffic between my private lab environment and the upstream household network.

My lab network uses the subnet `10.10.80.0/24`, with OPNsense assigned `10.10.80.1` as the LAN gateway and Proxmox assigned `10.10.80.10` for management.

Once I finish configuring DHCP, WAN connectivity, and the wireless access point, my desktop and other devices will be able to connect wirelessly to the lab network and access Proxmox, OPNsense, and my virtual machines without requiring a direct Ethernet connection to the Dell.

As I expand the environment, I can connect additional virtual machines, including Windows Server domain controllers and Windows clients, directly to `vmbr1`. These virtual machines will be able to communicate through the Linux bridge without requiring individual physical Ethernet ports.


---
title: My Homelab setup update 2024v1.1
date: 2024-05-06 13:00:00 +0400
categories: [homelab, setup]
tags: [homelab, setup, server, hardware, one-liter-pc, hp]
image:
  path: assets/img/posts/20240506/hp_elitedesk_800_g3_mini_thumbnail.webp
  lqip: data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/2wBDACgcHiMeGSgjISMtKygwPGRBPDc3PHtYXUlkkYCZlo+AjIqgtObDoKrarYqMyP/L2u71////m8H////6/+b9//j/2wBDASstLTw1PHZBQXb4pYyl+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj4+Pj/wgARCAAHAAoDAREAAhEBAxEB/8QAFQABAQAAAAAAAAAAAAAAAAAAAQT/xAAWAQEBAQAAAAAAAAAAAAAAAAABAAL/2gAMAwEAAhADEAAAAXLQX//EABUQAQEAAAAAAAAAAAAAAAAAABAT/9oACAEBAAEFApn/xAAVEQEBAAAAAAAAAAAAAAAAAAAQEf/aAAgBAwEBPwGH/8QAFhEBAQEAAAAAAAAAAAAAAAAAABES/9oACAECAQE/AdK//8QAFRABAQAAAAAAAAAAAAAAAAAAEDH/2gAIAQEABj8Cp//EABYQAQEBAAAAAAAAAAAAAAAAAAEAEf/aAAgBAQABPyEcBANv/9oADAMBAAIAAwAAABDb/8QAFhEBAQEAAAAAAAAAAAAAAAAAABEB/9oACAEDAQE/EIxH/8QAFhEBAQEAAAAAAAAAAAAAAAAAAREQ/9oACAECAQE/ELSY/8QAGRAAAgMBAAAAAAAAAAAAAAAAABEBITGR/9oACAEBAAE/EEajwWRULD//2Q==
---

Well, as someone who chose a Raspberry Pi 4 as their first homelab server machine, it did not take a lot of time for me to feel limited in what I could do in terms of self-hosting and experimenting with different Linux distros. Don't get me wrong, I still think it is a great little computer and use it to host lightweight stuff, but I wanted to start doing more exciting and hardware-demanding stuff, as well as trying out different Linux distros in a convenient way without wiping out my main deployment infrastructure and starting from scratch every time. Naturally, I became interested in virtualization: deploy and host your sandboxes where you can spin up virtual machines in a couple of minutes and bring them down as easily when you are done testing, and tinkering. Sounds great isn't it?

Unfortunately, a tiny SBC with an ARM-based CPU and 4 GB of RAM does not look all that mighty for this purpose, no matter how much I like it and how great and awesome it is. So, as a homelaber does, I went and bought yet another computer. This time more powerful and versatile. If you are also looking to start your self-hosted virtualization journey, you might find this blog useful.

## Goals and requirements

This time I wanted an x64-based CPU computer (ideally, 4 cores) with socketed RAM with support for at least 16 GB of capacity (very important for virtualization) and at least 2 storage drive slots. When it comes to the form factor it still felt kind of boring to just buy a full-size PC. I wanted to find something bigger than RPi 4, but maintainable and easily upgradable. Going small would also help with one other important criterion - power consumption. It would be great to have the machine consuming around 20 - 30 W under moderate loads because I was going to run it 24/7.

All that led me into a mini PC research rabbit hole yet again. This is how I discovered the 1L (liter) mini PCs. Those are computers having a case with a volume close to 1 liter, and usually having dimensions of around 200x200x40 (WxDxH) mm. They are usually known for being relatively easy to maintain and upgrade due to their popularity among businesses. Companies often buy whole fleets of those little thingies and being able to replace and configure the hardware easily is one of the main purchase determining factors. For that reason, those computers often come with at least 2 socketed RAM slots, socketed CPUs, and toolless 2.5" storage drive trays. Companies usually use those types of computers for several years, until they upgrade to a more modern and powerful fleet, releasing hundreds and thousands of 5 - 10-year-old used machines to the second-hand hardware market for people to pick them up as office and home computers for fairly low prices. Used and cheap? Say less!

Through my research, I found that there are 3 most popular mini PC series in this area: HP EliteDesk (ProDesk), Lenovo ThinkCentre, and Dell OptiPlex. The guys from [servethehome.com](https://www.servethehome.com/) conveniently call those 3 series combined as [TinyMiniMicro](https://www.servethehome.com/introducing-project-tinyminimicro-home-lab-revolution/).

In many cases, their configurations and hardware layouts seem to be very similar across different model tiers and product lines and in my research, I did not really with a decent reason to choose one over another, other than, of course, the hardware configurations themselves, price, and condition. At least, for my purposes that is.

## What I bought

By this point, it is probably obvious from the thumbnail that I went with [HP EliteDesk 800 G3 Mini (35 W)](https://support.hp.com/us-en/document/c05371240). The reason being no other than it was the best option I was able to find locally. I picked up a 7-year-old machine for around 100$, including a 320 GB 2.5" HDD drive, 8 GB of RAM, and a power adapter. There were dozens of similar offers available on eBay US for half that, but it made no difference after factoring in international shipping. My computer came with an i5-6500T. Here, the 'T' stands for low-powered CPU series designed for compact systems where cooling is challenging.

![Desktop View](assets/img/posts/20240506/i5-6500t-close-up.webp){: width="1049" height="662" }
_Intel i5-6500T in its socket_

You can find more details about this computer in the [dedicated article](https://www.servethehome.com/hp-elitedesk-800-g3-mini-ce-review-project-tinyminimicro/) by the same great guys at servethehome.com.

Here is a quick hardware overview:

- Intel i5-6500T: 4 cores, 4 threads, 2.5/3.1 (base/turbo) GHz frequency.
- Intel HD Graphics 530.
- 2x SODIMM DDR4-2400 RAM sockets (up to 32 GB supported).
- 1x 2.5" SATA slot.
- 1x M.2 drive slot for NVMe storage.
- 1x M.2 slot for the Wi-Fi and Bluetooth adapter.
- Dimentions: 177x175x34 (WxDxH) mm.
- Weight: 1.21 kg.

![Desktop View](assets/img/posts/20240506/motherboard_top_view.webp){: width="651" height="587" }
_HP EliteDesk 800 G3 Mini motherboard in all its glory_

Here is the list of hardware I ended up buying for the build (Apr 2024, shipping excluded):

| Item name    | Model                    | Details               | Quantity | Price (USD) |
| :----------- | :----------------------- | :-------------------- | :------- | ----------: |
| Mini PC      | HP EliteDesk 800 G3 Mini | i5-6500T, 35 W        | 1        |        ~100 |
| RAM          | Apacer ES.16G21.GSH      | SODIMM DDR4-3200 16GB | 1        |          44 |
| PCIe M.2 SSD | Patriot P300P256GM28     | 2280, 256 GB          | 1        |          28 |
|              |                          |                       | Total    |       ~ 172 |

Well, yet again this proves the point of single-board computers not being the most cost-effective option for self-hosting. If compared to [my RPi4-based server]({% post_url 2024-03-02-my-homelab-server-setup-v1.0 %}) my current addition, for virtually the same price, has a significantly more powerful CPU (4 cores, up to 3.1 GHz, vs 4 cores, 1.8 GHz) as well as not 3, 4, or even 5, but 6 times the amount of RAM (24 GB vs 4 GB) and a proper 1700 MB/s NVMe SSD vs a 2.5" SSD connected via a USB 3.0 port. In all fairness, in May of 2024, it probably makes more sense to compare this PC against an RPi 5, but even then the price-to-performance ratio still does not look much better. Said all that, I still believe that SBCs are a great starter or lightweight workload option for many people and purposes. Just not for virtualization, it seems. My PRi4-based server is still deployed, happily running mostly networking-related services.

![Desktop View](assets/img/posts/20240506/whole_system_open.webp){: width="738" height="651" }
_HP EliteDesk 800 G3 Mini in the upgraded hardware configuration_

Here is the upgraded hardware configuration:

- Intel i5-6500T.
- Intel HD Graphics 530.
- 24 GB RAM.
- 256 GB NVMe SSD: 1700 and 1100 MB/s sequential reads and writes, correspondingly.
- 1x 2.5" SATA slot vacant: will probably be used for a simple SSD-based NAS.
- 1x M.2 slot vacant: might consider replacing the Wi-Fi adapter with a 2242 M.2 SSD.

## What to look out for

Here you can find a couple of things I figured out as part of my experience that are important to consider before the purchase.

### Locked BIOS

Sometimes those mini PCs come with locked BIOSes. This is not exclusive to HP. It is very important to try and check before purchasing to make sure there are not going to be any issues when installing another OS or tinkering with virtualization support settings if needed.

### Fan always running

HP EliteDesk 800 G3 Mini's fan never comes to a stop when the computer is turned on. And it seems to be a feature, not a bug. It is possible to lower the default idle speed in BIOS, but the fan will still spin even if the CPU is at 20 - 30 degrees Celsius when the machine sits idle. The good thing I guess is that the fan itself is pretty quiet. However, I can still hear it just a little bit, but enough to get annoyed when sitting next to it a good chunk of a day, especially during the times when the house gets particularly quiet. I am currently waiting for a shipment that includes a new OEM replacement. Many community members shared that their fans became virtually silent after a replacement, which makes sense since mine is around 7 years old. The nice thing is that the replacement process is toolles.

### SSD mounting

More often than not those kinds of second-hand mini PCs come with no included rubberized mounting screws for 2.5" storage drives. Those are used for securing the drives to the metal bracket/tray thingy you can see in the photo above. Luckily, they are easy to find on Amazon, AliExpress, and other platforms for a couple of dollars per set. You might also be lucky to find them locally - did not work out that well in my case, unfortunately.

## Current software setup

I am currently running a bare-metal install of [Proxmox Virtual Environment](https://www.proxmox.com/en/proxmox-virtual-environment/overview), an open-source server management platform for enterprise virtualization. It is free with different enterprise support subscription tiers available. I feel great about this choice since all the basic stuff I am currently learning to do is very simple and user-friendly with a convenient web UI. In addition to running VMs, it can also run containers, even though I did not test it myself, since I deployed mine on a dedicated VM.

Virtual machines I am currently running (all open-source, free in general or for personal use):

- [Linux Debian 12](https://www.debian.org/News/2024/20240210) + Docker + [Portainer](https://www.portainer.io/) for hosting my containerized workloads.
- [Linux Debian 12](https://www.debian.org/News/2024/20240210) + Docker (via [CasaOS](https://casaos.io/)) + [Crafty Controller](https://craftycontrol.com/) + [PaperMC](https://papermc.io/) for hosting my Minecraft servers.

## What is next?

- Deploy a Proxmox cluster?
- Deploy a high-availability Kubernetes cluster just for the fun (sarcasm intensifies) of it?

## Wrap Up

If you are considering 1 Liter computers as your next step in homelabing, I strongly encourage you to do so! As we can see, these can be pretty powerful and versatile for the price and what they are. Mine has been serving me well for the last month and was been nothing other than a pleasure to work with. Virtualization with Proxmox runs flawlessly. The community support is just awesome, as well as the community itself. Feel free to reach out to me in case of any questions, to share feedback, or just your progress.

## Useful URLs

- <https://www.servethehome.com/introducing-project-tinyminimicro-home-lab-revolution/>
- <https://www.servethehome.com/hp-elitedesk-800-g3-mini-ce-review-project-tinyminimicro/>
- <https://support.hp.com/us-en/document/c05371240>
- <https://www.proxmox.com/en/proxmox-virtual-environment/overview>
- <https://www.debian.org/News/2024/20240210>
- <https://casaos.io/>
- <https://craftycontrol.com/>
- <https://papermc.io/>
- <https://www.portainer.io/>
- <https://docs.docker.com/engine/>

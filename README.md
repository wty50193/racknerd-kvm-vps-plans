# kvm vps hosting from $1.83 a month: which RackNerd plan runs Docker, what KVM actually isolates, and the console mix-up to avoid (full plan table)

KVM VPS hosting sits at roughly $3 to $7 a month at most providers. RackNerd's entry point is $21.99 for a year, prepaid: 1 vCPU, 1 GB RAM, 20 GB of RAID-10 SSD and 3 TB of transfer, which works out to about $1.83 a month. Every plan on RackNerd's specials page is annual, unmanaged, and priced by RAM tier, so the plan you want is decided by what you intend to install, not by how little you can get away with spending.

Below: the full plan table, the RAM cutoffs for the one-click apps RackNerd ships, what KVM isolation changes next to OpenVZ, and a term mix-up that sends people to support with the wrong question.

> **Most readers should take the 2 GB special at $35.99 a year.** 2 vCPU, 2 GB RAM, 35 GB SSD and 5 TB of transfer covers a small site, a Discord bot, or one self-hosted app such as n8n or Coolify, both of which want 2 GB to run comfortably. Move to 4 GB at $59.99 once Docker, a database and a reverse proxy share the same box.
> 👉 [See the current RackNerd KVM VPS specials and prices](https://bit.ly/RacKnerd)

## The full RackNerd KVM VPS lineup

| Plan | Specs | Price (annual, prepaid) | Best for | Order |
| --- | --- | --- | --- | --- |
| 1 GB Special | 1 vCore / 1 GB RAM / 20 GB SSD / 3 TB | $21.99/yr (~$1.83/mo) | VPN endpoint, uptime monitor, one low-traffic site | [Order the 1 GB special](https://bit.ly/RacKnerd) |
| 2 GB Special | 2 vCore / 2 GB RAM / 35 GB SSD / 5 TB | $35.99/yr (~$3.00/mo) | n8n, Coolify, Discord bot, a single small site | [Order the 2 GB special](https://bit.ly/RacKnerd) |
| 4 GB Special | 3 vCore / 4 GB RAM / 60 GB SSD / 7 TB | $59.99/yr (~$5.00/mo) | Docker plus a database, Ollama or LibreChat front-ends | [Order the 4 GB special](https://bit.ly/RacKnerd) |
| 6 GB Special | 6 vCore / 6 GB RAM / 100 GB SSD / 12 TB | $89.99/yr (~$7.50/mo) | Several containers, heavier builds, staging plus production | [Order the 6 GB special](https://bit.ly/RacKnerd) |
| 8 GB Special | 7 vCore / 8 GB RAM / 150 GB SSD / 20 TB | $119.99/yr (~$10.00/mo) | OpenClaw, multi-app self-hosting, a full home-lab stack | [Order the 8 GB special](https://bit.ly/RacKnerd) |

Every plan carries full root access, a 1 Gbps port, RAID-10 SSD storage and RackNerd's own `ctrl` control panel. OS templates include CentOS, Rocky Linux, AlmaLinux, Fedora, Debian and Ubuntu, with custom ISO installs handled through a support ticket. Prices above are the listed annual rates at the time of writing; the order page is the authority if they change.

## What KVM gives you that OpenVZ and container VPS don't

KVM is built into the Linux kernel, which turns the host's Linux into a hypervisor. Each VM gets its own virtual hardware and its own kernel. Three practical consequences follow, and they decide most purchase decisions:

**You can run any OS.** Windows Server with your own licence, a distribution the host doesn't template, a BSD. OpenVZ and other container-based plans share the host kernel, so Linux is the only option and your choice of kernel version belongs to the provider.

**You control the kernel surface.** Load your own kernel modules, set your own sysctls, run your own container runtime. Container hosting can't offer that, because those settings would affect every tenant on the node.

**Your resources are yours.** With KVM, the RAM and CPU you bought are allocated to your VM. On container plans, a noisy neighbour on the same node shows up as latency on your side. OVHcloud's product page states KVM VPS is faster than a standard VPS for exactly this reason, and while that is a vendor claim rather than a benchmark, the architecture behind it is real and worth understanding when you compare prices.

RackNerd's plans are unmanaged KVM, so the operating system, the firewall rules and the application stack are your job. Docker is one click from `ctrl` via Portainer, or install it yourself over SSH. If Docker plus Postgres plus a reverse proxy is the plan, 2 GB gets tight fast.
👉 [Pick the 4 GB special if Docker and a database share the box](https://bit.ly/RacKnerd)

## KVM the hypervisor vs the KVM console

Two different things share the name, and the confusion is common enough that OVHcloud separates them explicitly on its own KVM VPS page. KVM the hypervisor is the virtualisation layer. A KVM console is a Keyboard, Video, Mouse remote session, the out-of-band access that behaves as if you were sitting in front of the machine.

Why it matters in practice: you edit `/etc/fstab` badly, the VPS doesn't come back up, and SSH is gone. The only way in is the console. RackNerd includes an HTML5 VNC console inside `ctrl` alongside reboot, OS reinstall, root password reset, firewall rules and plan upgrades, so recovery doesn't depend on opening a ticket and waiting. When you compare providers, check whether the console exists in the panel and whether you can reinstall an OS yourself. A host that gates both behind support is a host you'll wait on at the worst possible moment.

## The real cost: annual prepay, and what year two looks like

There is no monthly billing on these specials. The 1 GB plan is $21.99 for twelve months, the 8 GB plan is $119.99, and both are paid up front. Divide by twelve and the effective monthly rate is $1.83 to $10.00.

That changes the comparison with monthly-billed hosts. Rad Web Hosting advertises KVM VPS from $3.19 a month with its smallest listed configuration at 1 vCPU and 256 MB RAM. InstantNode's Loki plan is €4.55 a month for 2 vCPU and 4 GB. Tonic VPS starts at €3.60 a month in Germany. Liquid Web's memory tier starts at $17 a month for 1 vCPU and 4 GB. On a twelve-month basis, RackNerd's 2 GB plan at $3.00 a month equivalent lands below all of those, and the trade is that you commit the whole year at once and get no monthly exit.

Year two is a separate decision. A LowEndTalk reviewer who bought a Black Friday plan noted that RackNerd kept pricing from past promotions in place, which is the behaviour you want from an annual-prepay host. Confirm the renewal rate on the order page before you pay, because with annual billing there is no month-to-month fallback if the second-year price doesn't suit you.

## Where RackNerd is the wrong buy

**You want managed hosting.** These are unmanaged KVM VPS. Support handles the platform: provisioning, network, hardware, the panel. Nobody is going to debug your Nginx config.

**You need clean IPs for mail.** The same LowEndTalk reviewer reported receiving IPs with existing blocklist history and described the infrastructure as unstable enough that he wouldn't use it for production. That is one customer's account, not a measured failure rate, but it's the kind of thing to test before you migrate a mail server. Ask support about port 25 policy and IP reputation before ordering if outbound mail is part of the plan.

**You need IPv6 guaranteed.** Reports on IPv6 availability differ by location and by plan. Confirm with support rather than assuming.

**You need a refund window.** I couldn't verify a refund policy from RackNerd's own pages, and the reviewer above said he found the refund process difficult or unavailable. Treat an annual prepay as final until support tells you otherwise in writing.

**You need dedicated hardware.** A VPS is a slice of a shared server with isolated resources. If you need the whole machine, that's a dedicated server and a different budget entirely.

## Before you pay

1. **Renewal price.** Check that year two costs what year one cost.
2. **RAM against the app.** RackNerd's own documentation sets floors for the one-click catalogue: n8n and Coolify run from 2 GB, Ollama, LibreChat, AnythingLLM and Hermes Agent want at least 4 GB, OpenClaw wants 8 GB. Buy below the floor and you'll be reinstalling to a bigger plan within a week.
3. **Location and latency.** US locations include Los Angeles, San Jose, Seattle, Dallas, Chicago, Atlanta, New York, New Jersey and Ashburn, plus Amsterdam and France. Not every location is stocked for every plan.
4. **Custom ISO and OS reinstalls.** Standard templates are one click in `ctrl`; anything else goes through a ticket, so plan the first install accordingly.
5. **Backups.** Nothing in the plan list reads as an automatic backup service. Assume you're responsible for your own snapshots and off-server copies.

## Which plan fits your workload

One low-traffic site, a WireGuard endpoint, an uptime monitor, a bot that idles: 1 GB at $21.99 a year is enough, and paying more buys you nothing you'll notice.

A single self-hosted app with a web interface plus a small site: 2 GB at $35.99. This is the tier most people should start on, because it clears the 2 GB floor for n8n and Coolify and leaves headroom for a reverse proxy.

Docker with a database, or a local model front-end: 4 GB at $59.99. Below this, swapping starts the moment Postgres and your app both get busy.

A stack of containers, staging next to production, or OpenClaw: 8 GB at $119.99. The 6 GB tier at $89.99 is the compromise if you're close but not certain.

Two things to settle before checkout: the renewal rate on your chosen plan, and whether the location you want has stock and IPv6 for your use case.

👉 [Start with the 2 GB RackNerd special at $35.99 a year](https://bit.ly/RacKnerd)

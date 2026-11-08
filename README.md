# Dedicated Servers: What They Cost, When a VPS Stops Being Enough, and Which Spin Servers Config Fits (Full Plan Comparison)

Dedicated server pricing splits into two markets that barely overlap. A committed European contract starts at €4.74/month (Scaleway's Dedibox Start, which requires a 12-month term). Month-to-month bare metal in the US starts around $79/month, which is where Spin Servers' San Jose catalogue begins. Both prices buy an entire physical machine. Contract length, hardware generation and how much administration you take on explain most of the gap.

You need one when a workload stops fitting inside a slice of someone else's hardware: sustained CPU or disk pressure, a compliance rule that wants physical isolation, or software licensed per socket or per core. Everything below covers the decision in that order: what the machine costs, what it changes, how it differs from VPS, cloud and "bare metal", and what to confirm before you pay.

> **Quick pick:** a single busy site, VPN endpoint, game server or build machine with a working set under 64 GB fits the entry San Jose configuration, listed from **$79/month** with 12 cores/24 threads, 64 GB RAM, a 240 GB SSD and 30 TB of transfer. Teams virtualising or running many containers should look at the 256 GB RAM box instead.
> 👉 [See the current San Jose configurations and prices](https://bit.ly/Spinservers)

## Spin Servers plans and configurations

Spin Servers sells bare metal in two US locations, Dallas, Texas and San Jose, California. Every listing below includes 30 TB of monthly transfer on a 10 Gbps port (San Jose machines carry two 10 Gbps ports), one IPv4 address and instant auto-provisioning. Stock rotates as machines are sold and replaced, so the order page is the only authoritative price for a given configuration.

| Configuration | CPU | RAM / storage | Price (USD, monthly) | Order |
| --- | --- | --- | --- | --- |
| Xeon E3-1280 v5, Dallas | 4C/8T, 3.7 GHz turbo | 32 GB DDR4 ECC · 1 TB M.2 NVMe | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| Dual Xeon E5-2630L v3, Dallas | 16C/32T, 2.9 GHz | 128 GB DDR4 ECC · 1 TB NVMe | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| 12C/24T, San Jose | 12C/24T, 2.6–3.1 GHz | 64 GB DDR3 ECC · 240 GB SSD | **$79/mo** (entry) | [Order the entry box](https://bit.ly/Spinservers) |
| Xeon E3-1280 v5 (SJC-205), San Jose | 4C/8T | 32 GB · 1 TB NVMe | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| 16C/32T, San Jose | 16C/32T, 1.8–2.9 GHz | 64 GB DDR4 ECC · 1.6 TB SSD | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| 16C/32T NVMe, San Jose | 16C/32T, 1.8–2.9 GHz | 64 GB DDR4 ECC · 1.6 TB NVMe | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| 24C/48T, San Jose | 24C/48T, 1.8–2.5 GHz | 64 GB DDR4 ECC · 1.6 TB SSD | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| 16C/32T, 256 GB, San Jose | 16C/32T, 1.8–2.9 GHz | 256 GB DDR4 ECC · 2 × 1.6 TB SSD, hardware RAID | Per-config quote | [Check this config](https://bit.ly/Spinservers) |
| 32C/64T, 512 GB, San Jose | 32C/64T, 2.1–3.0 GHz | 512 GB DDR4 ECC · 2 × 3.84 TB SSD, RAID with battery-backed cache | Per-config quote | [Check this config](https://bit.ly/Spinservers) |

Two caveats worth knowing before you budget. First, most of these boxes are quoted individually rather than listed at a flat rate, and the price moves with location, hardware and what happens to be in stock. A September 2026 snapshot of the same catalogue put the four headline tiers at $79, $99, $149 and $679 per month, month-to-month with no setup fee, which gives you the shape of the range: entry boxes under $100, high-memory machines in the hundreds. Second, the service is self-managed bare metal. Root access, Linux/Windows/ESXi auto-install and IPMI or iKVM remote management are part of the package; someone patching the kernel is not.

## What a dedicated server actually changes

A dedicated server is one customer to one physical machine. Nothing is partitioned, nothing is oversubscribed, and no hypervisor sits between your workload and the silicon. That produces three practical differences.

**Performance stops depending on strangers.** CPU, RAM, storage and bandwidth are yours in full. On a mid-range VPS, a noisy neighbour doing a large database import can drag your disk latency up even though your own allocation is untouched. On a dedicated box, the only workload competing with yours is yours.

**You get the底层 layer.** Port configuration, kernel parameters, RAID layout, filesystem choice, the whole monitoring stack: all of it is yours to set. Managed hosts rarely let you change that much, and VPS providers usually restrict kernel modules and custom images.

**Hardware isolation answers questions that shared hosting cannot.** Financial and medical workloads often sit under rules that require physical separation or an auditable boundary around the data. A single-tenant machine gives you fewer entry points and a smaller blast radius; a compromise on a shared server can reach every tenant on it.

servers.com's guide to dedicated hosting frames the buying signals as four questions, and they hold up: is your current host choking on CPU-heavy or I/O-heavy work; are traffic peaks producing visible lag; are security or compliance requirements unmet; do you want to customise the stack without arguing with a host's control panel. Two yeses is a reasonable trigger to move.

👉 [Browse what Spin Servers has in Dallas and San Jose right now](https://bit.ly/Spinservers)

## Dedicated servers vs VPS vs shared vs public cloud

The short version: shared hosting puts hundreds of tenants on one machine, VPS and cloud slice a machine with a hypervisor, and a dedicated server hands you the machine. AWS's own comparison page is a useful reference here because it sells both sides.

|  | Dedicated server | VPS |
| --- | --- | --- |
| Resource allocation | Whole machine: every core, DIMM and disk | A partitioned slice with fixed limits |
| Scaling | Add hardware or another server; costs money and time, and may mean downtime | Reallocate resources in a panel, within the host's limits |
| Performance | Maximum, no virtualisation overhead | Comparable for smaller workloads |
| Customisation | Any OS, kernel or hypervisor | Restricted by what the host permits on shared hardware |
| Cost | Higher, fixed monthly | Flexible, pay for the slice you use |
| Security and compliance | Full control of hardening; isolation can satisfy strict rules | Effective, but may not meet complex compliance requirements |

The scaling row deserves emphasis because it is where dedicated hosting stops being convenient. Growing a VPS is a slider. Growing a dedicated server means physically adding disks or RAM, or renting a second machine and clustering. Both take money and calendar time, and depending on how the host handles it, an interruption.

The shared and cloud tiers have their own ceilings. Shared hosting leaves you almost no configuration room and exposes you to whatever the other tenants are doing. Public cloud gives you elasticity and a broad service catalogue, but instances still run on shared hardware, so you inherit multi-tenant performance variance and a security model you largely do not control. AWS's comparison page also states plainly that dedicated servers are "expensive and often unnecessary for most organizations," arguing that modern hardware does far more than host one website and that a VPS avoids overprovisioning. That is a fair point from a vendor that profits either way, and it is the reason the entry tier of any dedicated catalogue is the only one worth starting with.

## Dedicated, bare metal, Dedicated Hosts, Dedicated Instances: the differences that cost money

The vocabulary overlaps badly, and the distinctions matter most when software licensing is involved.

**Bare metal** means direct hardware access with no hypervisor or cloud software preloaded. **Dedicated server** is the broader term and may include a provider's virtualisation layer running on top, which is exactly what Amazon EC2 Dedicated Hosts are: a physical server with EC2 capacity reserved for you, where you can bring existing per-socket, per-core or per-VM licences for Windows Server, SQL Server, SUSE Linux Enterprise or Oracle, control where instances are placed, and leave host maintenance to the provider.

**EC2 Dedicated Instances** are a different product, and the details trip people up. They are VMs on hardware dedicated to a single customer, with AWS handling placement and capacity. Two limitations matter: Dedicated Instances may share hardware with other instances from the same AWS account that are not Dedicated Instances, and the architecture does not support per-machine software licensing. If your reason for going dedicated is reusing a per-socket SQL Server licence, Dedicated Instances will not get you there. Dedicated Hosts, or a conventional dedicated server where you install the hypervisor yourself, will.

The general rule: per-socket and per-core licensing only makes sense on hardware you control at the physical level. If a licence audit is part of your calculation, confirm the licensing model before the hardware model.

## Managed or unmanaged?

Most dedicated hosting is unmanaged by default. You get root access, a machine, power and network, and you handle operating system updates, security patches, backups and monitoring yourself. Managed hosting adds provider-side patching, updates, monitoring and support, at a price that usually tracks the amount of hand-holding.

servers.com's guide makes a point that saves time during research: most good hosting providers specialise in one model, not both. A provider built around instant self-service provisioning is not the one to ask for a fully managed stack, and a managed provider will rarely match the price of an unmanaged box.

The decision comes down to what you want to own. A team with in-house operations, or a workload where kernel tuning and hardening are part of the value, should stay unmanaged and put the savings into hardware. A team without a sysadmin should price managed hosting into the comparison before choosing a machine, because a cheap unmanaged box plus an unplanned contractor is more expensive than a managed plan.

Spin Servers sits firmly on the self-managed side: root access, OS auto-install, remote console access and private Layer 2 networking between your machines. If you want someone else to apply kernel patches at 2 a.m., this is the wrong catalogue.

👉 [Compare the self-managed bare-metal configurations](https://bit.ly/Spinservers)

## What the monthly fee pays for, and where cheap headline prices hide the cost

A dedicated server bill covers data centre floor space, power and cooling, on-site technicians and network engineers, the hardware itself and its replacement cycle, operating system licensing and installation, firmware fixes, and in many cases DDoS mitigation. None of that is optional, which is why the genuinely cheap dedicated listings tend to have a condition attached.

Take Scaleway's Dedibox line. The €4.74/month Start price requires a 12-month commitment. The €29.74/month Pro and €110.49/month Core prices require 36-month commitments. Over the first year, the Start box costs about €57; a month-to-month US machine at $79 costs about $948. The gap is mostly term length and hardware class, not a secret discount, and it should tell you what to compare first: specs, then term, then the monthly number.

For a sense of what a mature provider bundles as standard, OVHcloud advertises deployment in 120 seconds, 46 data centres, public bandwidth from 1 to 220 Gbps, unlimited traffic (excluding its Asia-Pacific locations), anti-DDoS protection and 500 GB of external storage included, with ISO/IEC 27001, 27017, 27018, 27701, SOC 1–2 Type I and HDS certifications available as options in some locations. Those are the questions to ask any host: what is included, what is an add-on, and which certifications actually apply to the facility your machine sits in.

## FAQ

**Do I install the operating system myself?**
On an unmanaged server, yes, though most providers offer templates. Spin Servers lists auto-install for Linux distributions, Windows and ESXi, plus IPMI/iKVM access if you prefer a manual install.

**Do I need Linux skills?**
For an unmanaged box, you need someone who can patch a kernel, configure a firewall and restore a backup. If nobody on the team can, choose managed hosting or stay on a VPS until you can.

**Can I run Windows or VMware?**
Both appear in the standard OS lists, and virtualisation is normally allowed on single-tenant hardware. Windows licensing is an extra cost, and some providers charge for the licence monthly.

**Is DDoS protection included?**
It varies. OVHcloud includes anti-DDoS on its dedicated range, and Spin Servers advertises a DDoS-protected, multihomed network. Ask what the mitigation does to legitimate traffic during an attack before you rely on it.

**How fast is delivery?**
Spin Servers lists its in-stock configurations as instant or auto-provisioning. Custom builds take longer; OVHcloud cites 120 seconds as its floor for standard configurations. If you need a specific CPU and disk combination, expect to wait.

**What happens after 30 TB of transfer?**
Ask before ordering. Bandwidth overage policies range from throttling to per-TB charges, and a 10 Gbps port can burn through 30 TB in under seven hours of sustained saturation.

**Can I get a refund?**
Spin Servers sells month-to-month, and third-party reviews of the service note that monthly plans generally carry no refund policy. Confirm the terms on the order page rather than assuming a trial window.

## How to choose

Match the machine to the workload, not to the future you hope for.

- One busy site, a VPN endpoint, a game server or a CI box under 64 GB of working set: the entry San Jose configuration at $79/month is the whole answer. More cores will not make a WordPress site faster.
- A virtualisation host running several VMs, or a container platform: the 256 GB RAM machine with dual 1.6 TB SSDs and hardware RAID, since memory is what limits VM density and RAID is what keeps you alive when a disk dies.
- Databases, analytics or machine learning with large working sets: the 32-core, 512 GB box. Disk throughput and RAM capacity matter more than core count here.
- Compliance-driven isolation: any of the above, plus your own hardening and an audit of the specific facility. A provider's certification badge does not transfer to your application.
- No in-house sysadmin: none of the above. Price managed hosting first.

Before you pay, confirm five things: the commitment length, whether the plan is managed or self-managed, whether a setup fee applies, the refund window, and what happens when you exceed the bandwidth allowance. Get those in writing, then pick the smallest machine that clears your current peak by a comfortable margin.

👉 [Check current Spin Servers stock and per-configuration pricing](https://bit.ly/Spinservers)

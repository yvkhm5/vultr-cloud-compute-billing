# Cloud VPS from $2.48/mo: which Vultr tier fits your workload, what the $300 credit really covers, and the billing fine print worth reading first

Vultr's Cloud Compute entry instance (1 vCPU, 512 MB) lists at **$2.48/mo** and bills at $0.0034/hr. The $300 offer is a promo code, **FLY300VULTR**, that drops $300 of credit into a brand-new account. It's new customers only, it applies to select products only, and it can't be combined with any other Vultr offer.

Two numbers decide most purchases here: the hourly rate that lets you test something for a weekend, and the monthly rate you'll actually pay once you stop experimenting. Below are the plan anchors, the credit conditions read line by line, and the four billing questions that separate a $20 bill from a $60 one.

> **Quick pick:** a single WordPress site, a small Node or Laravel API, or a staging box that mirrors production all fit the **2 vCPU / 4 GB Cloud Compute instance at $20/mo**, which ships 80 GB of local SSD and 3 TB of transfer. If you're spending the $300 credit on it, that's 15 months of runway on one box. 👉 [Start with the 2 vCPU / 4 GB instance](https://bit.ly/VuLtr)

## The Cloud Compute price sheet, as it stands

| Instance | vCPU / RAM | Monthly | Hourly | Order |
| --- | --- | --- | --- | --- |
| Cloud Compute `vc2-1c-0.5gb-v6` | 1 / 512 MB | $2.48 | $0.0034 | [Deploy the entry instance](https://bit.ly/VuLtr) |
| Cloud Compute, 2 vCPU / 4 GB | 2 / 4 GB | $20.00 | — | [Deploy 2 vCPU / 4 GB](https://bit.ly/VuLtr) |

The $20 row is the one Vultr itself uses as a reference point, and it includes 80 GB of local SSD plus 3 TB of transfer. Vultr's own comparison against a Google Compute Engine instance of the same shape puts the gap at $272.46/mo versus $20, and $240 of that difference is 3 TB of egress billed at $0.08/GB. Treat the comparison as vendor math, but the lesson holds: on metered clouds, transfer is often the line item that decides the bill.

Above this level the catalog gets deep. Vultr's Cloud Compute sheet runs to 122 configurations, from the $2.48 entry box up to $2,102.40/mo for a 96 vCPU / 384 GB GPU instance, split across Regular, High Frequency, High Performance, and Optimized (CPU, memory, storage) families. Most readers never need past the second row. 👉 [See the full price sheet](https://bit.ly/VuLtr)

Three things that table doesn't show:

- **On-demand instances bill hourly.** Spin up a box for a migration test on Friday and destroy it Sunday, and you owe cents, not a month. Reserved commitments of one or three years exist if you want a lower hourly rate on something permanent.
- **Deployment is fast enough to be disposable.** Vultr advertises under 60 seconds to a running instance, with a one-click app and OS marketplace and a documented API. That matters more for CI runners and test environments than for a production site.
- **32 locations.** Pick the region closest to your users; latency from Singapore to a Frankfurt box is a self-inflicted problem.

## The $300 credit, condition by condition

Vultr's coupon page currently lists four codes, and they don't all work the same way:

| Code | What it does |
| --- | --- |
| FLY300VULTR | $300 free credit |
| FLYTWOHUNDRED | $200 free credit |
| 250VULTRFLY | $250 free credit |
| VULTRMATCH | Vultr matches your first deposit dollar for dollar, up to $100 |

All four carry identical fine print: promotional credit applies to select products only, cannot be combined with any other offers, restrictions and terms apply, new customers only.

So the first decision is that you pick one. If you're opening a fresh account, $300 beats $200 and $250. The interesting comparison is FLY300VULTR against VULTRMATCH, because they're different instruments. The $300 is promotional balance that gets consumed as you deploy resources; it isn't cash, it isn't a discount on a future invoice, and it doesn't come back when it's gone. VULTRMATCH adds $100 on top of $100 of your own money, which is smaller but sits alongside a real balance you control.

Which one wins depends on whether you were going to fund the account anyway. If the answer is no, take the $300. If you were already planning to put $100 in, the match gives you a $100 bonus with no consumption deadline attached to it.

What $300 actually buys, at listed rates:

- 15 months of one 2 vCPU / 4 GB instance at $20/mo
- Roughly 120 months of the $2.48 entry box
- Three months of a five-node cluster built from $20 instances, which is enough time to break a Kubernetes setup repeatedly and learn from it

The coupon page doesn't publish an expiry window, and promotional balances usually do expire. Check the Billing panel the day the credit lands and note the date before you architect a year around it. 👉 [Check the current credit codes and terms](https://www.vultr.com/coupons/?ref=9738262-9J)

## Matching a tier to what you're actually running

Config advice from providers tends to be self-serving, so here are the breakpoints RamNode publishes for its own sizing, which match what most people see in practice:

**2 vCPU / 3 GB** handles a handful of apps at light-to-medium traffic, a Node or PHP app with a small database alongside, a static React site, or a personal VPN. WireGuard and Tailscale are light enough that the smallest tier covers a household.

**4 vCPU / 8 GB** is the floor for anything with a database under real concurrency. Multiple WordPress installs with plugins, caching and monitoring; a production Node API with Postgres; SSR or Next.js under load; several dev environments at once. Docker image builds and parallel deploys are CPU-bound, and 2 vCPU starts to hurt there.

**Kubernetes wants 4 vCPU / 8 GB minimum** for a control-plane node or any workload you'd call real. At 2 vCPU / 3 GB you're limited to minikube or single-node k3s experiments.

The honest read on the entry tier: 512 MB of RAM runs a VPN endpoint, a bot, a jump box, or a Linux box you're learning on. It does not run a database and a web server at the same time without swap trouble. If you're unsure, the $20 instance is the cheapest tier where you won't spend a weekend tuning OOM behaviour.

## Four billing questions to settle before you pay

**Is the advertised price monthly, or a long-contract average?** Contabo lists plans "from €4.40/month," and the same page's fine print states the displayed price is the effective monthly rate for a 24-month subscription including applicable taxes. Its Cloud VPS 4 also carries a one-time setup fee on new monthly-billing orders. A "from" figure and a month-to-month figure can differ by a wide margin, so check which one you're looking at.

**Does the price include a dedicated IPv4?** RamNode's Lite tier starts at $2/mo with IPv6 only; adding IPv4 costs another $2/mo, which doubles the bill on the smallest plan. Confirm what's bundled before comparing headline prices across providers.

**What does "unlimited traffic" mean in practice?** Contabo offers unlimited inbound and outbound transfer with a fair-usage policy and port speeds from 200 Mbit/s to 1 Gbit/s. OVHcloud advertises unlimited traffic with bandwidth scaling up to 3 Gbps depending on model. Neither number is a lie, and neither tells you what happens at 3 a.m. when your port is throttled. Vultr takes the metered route instead: 3 TB included on the $20 instance. Know your expected egress and price the overage before you migrate.

**How much DDoS protection is included?** Liquid Web's standard protection covers Layer 4 attacks up to 2 Gbps, with an advanced tier past 10 Gbps sold separately. "DDoS protection included" is true for nearly every provider and means something different at each one.

## Who shouldn't buy this

If you want a website and not a server, a managed plan is the better spend. Vultr hands you root access and a control panel, and everything above the OS is your problem. Patching, backups, and the 2 a.m. restart are yours.

If compliance paperwork is a requirement, verify certifications directly with the provider you're considering. Liquid Web states HIPAA, PCI, SSAE, and Privacy Shield compliance and runs its own data centres. Whether Vultr fits a specific regulated workload is a question for its compliance documentation, not for a pricing article.

If you need someone to call, check support terms before you deploy rather than after.

## FAQ

**Can I pay month to month?**
On-demand instances bill hourly with no long-term contract, so there's no minimum term to sign. Reserved commitments are optional and separate.

**Can I stack FLY300VULTR with VULTRMATCH?**
No. The coupon page states promotional credit cannot be combined with any other offers. One code per new account.

**What happens when the credit runs out?**
Billing reverts to your payment method at standard rates. The credit doesn't renew, and the $20/mo instance keeps costing $20/mo.

**Can I resize later?**
Plan for it. Moving within an instance family and moving between families (Regular to High Frequency, for example) are different operations, so confirm the upgrade path for the exact instance you pick before you build on it.

**Should I start on the smallest tier and grow?**
Only if growing in place is cheap. For anything with a database, starting at 2 vCPU / 4 GB avoids a migration two weeks in.

## What to pick, by situation

Running one site or a small API for yourself: the **2 vCPU / 4 GB at $20/mo** is the sane default, and $300 of credit covers 15 months of it. A VPN, a bot, or a Linux sandbox: the **$2.48 entry instance**, no question. A production API with Postgres and background jobs: stay at 2 vCPU / 4 GB or step up, and don't put the database on the same box if you can avoid it. Learning Kubernetes: budget for at least 4 vCPU / 8 GB per node, and use the credit to run a small cluster for a few months.

Two things to confirm at checkout: the region closest to your users, and whether the promo code registered in Billing before you deploy anything. Retroactive claims on a running account are painful. 👉 [Deploy the Vultr tier you landed on](https://bit.ly/VuLtr)

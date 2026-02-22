---
layout: post
title: "How Garmin Engineered Multi-Week Battery Life Into a Watch"
date: 2026-02-22 23:00:00 +0800
categories: thoughts
---

The smartwatch market is a showcase of contrasts. Most smartwatches last a day or two between charges, but with Garmin, some models display an infinity symbol where the battery percentage should be. An actual infinity symbol, meaning the watch is really gaining energy faster than it's spending it.

That kind of gap comes from a stack of engineering decisions that compound on each other: the glass, the solar layer beneath it, the display technology, the choice of MCU, the firmware's power management – all of it working together in a package that weighs ~60 grams.

This post unpacks two pillars of how Garmin keeps winning the battery war: **the glass** (how energy gets in) and **the power architecture** (how little energy gets used). It's not a product review, and it's not the usual rehash of spec sheets that you're probably tired of reading. Think of it more as an engineering analysis – built on teardown findings, component datasheets, patent filings, and field test data.

We'll trace the solar tech from its first appearance in 2019 through the current generation, crack open the watch to see what silicon is actually running the show, and look at where this technology is headed – and whether anyone else can catch up. If, like me, you have ever wondered what really goes on under that "sapphire glass", then this post is for you.

![Garmin Infinite Battery icon](/assets/images/garmin-infinite-battery.webp)

---

## From Fenix 6 to Instinct 3

The story begins with an acquisition. In the years leading up to 2019, Garmin quietly acquired SunPartner Technologies, a French company specializing in transparent photovoltaic surfaces, as it was nearing bankruptcy. The goal was direct: integrate solar energy capture directly into a watch face without impairing the display. The result shipped in the Fenix 6 series – the first Garmin watch with solar charging.

The first design was clever but came with visible tradeoffs. A single semitransparent solar layer was bonded between the display and the cover glass, essentially creating a photovoltaic sandwich. This layer used a two-density approach: highly efficient solar cells around the display edge captured energy at near-100% photovoltaic efficiency, while a much less efficient array – roughly 10% covered the main screen area. The logic was sound: maximize harvesting at the perimeter where it doesn't interfere with readability, and capture what you can from the rest without blocking the display entirely.

The solar charging feature did its job, but you couldn’t miss its presence. The layer beneath the screen gave the display a subtle reddish tint, especially noticeable if you caught it in the right light. It didn’t ruin the experience, but it was a visible sign that Garmin was still polishing up a brand-new technology.

<!-- ![Garmin fenix 7 Pro Saphire Solar (2023) vs Garmin Fenix 8 Saphire Solar](/assets/images/garmin-fenix-7_vs_garmin-fenix-8.webp) -->

<figure>
  <img src="/assets/images/garmin-fenix-7_vs_garmin-fenix-8.webp" alt="Garmin fenix 7 Pro Saphire Solar (2023) vs Garmin Fenix 8 Saphire Solar">
  <figcaption>Garmin fenix 7 Pro Saphire Solar (2023) vs Garmin Fenix 8 Saphire Solar</figcaption>
</figure>


**From 2019 through 2024, Garmin iterated**. Solar started popping up everywhere in Garmin’s lineup – first in the Instinct Solar, then the Instinct 2 Solar, the Enduro series for endurance diehards, and even on Edge bike computers. Each release brought solar tech to a new crowd at a different price, but under the hood, the tech didn't change: same dual-density solar panels, same hidden layer between glass and display, and that reddish hue.

That changed in late 2024 with the Fenix 8 Solar and Enduro 3.

These were the first Garmin watches to ship a meaningfully redesigned solar architecture since the earliest Fenix 6. The changes were structural, not incremental. Garmin relocated the solar component entirely off the main screen area – instead of a semitransparent layer sitting over the display pixels, the solar cells now lived exclusively on the non-screen bezel portions of the watch face, packed at a much higher density. The red tint was gone. Paired with a Power Sapphire lens – sapphire crystal, the second-hardest transparent material after diamond – the Enduro 3 pushed the marketed battery life to 90 days with solar in smartwatch mode and 320 hours of GPS tracking.

[Garmin claimed up to 5× the solar charging power of the previous generation](https://ph.garmin.com/news/press-release/news-2020-aug-solar/), and actual data confirmed it. The architecture delivered those gains where they mattered most: sustained GPS tracking for people who actually need multi-day battery life in the field.

In January 2025, this Gen 2 solar technology trickled down from the premium tier into the Instinct 3 Solar – the first meaningful solar hardware upgrade for that series. Garmin paired the new solar architecture with a redesigned MIP display, and the results were awesome. Where the Instinct 2 Solar managed 48 hours of GPS-only tracking with solar, the Instinct 3 Solar [pushed that to 130 hours](https://www8.garmin.com/manuals/webhelp/GUID-31D23DBB-57C2-4DF7-A0C9-8D1A00AB4BE7/EN-US/GUID-BCA44331-7CAC-4734-9222-6066B05C32AB.html). In Max Battery GPS mode, it achieved what Garmin officially labels "unlimited" battery life with adequate sun exposure – and independent reviewers confirmed it, logging burn rates under 1% per hour in high-drain GPS modes. A watch that costs a fraction of the Fenix 8 Solar, running the same solar tech, hitting "unlimited" in multiple modes. That's when you know a generation jump is real

| Year | Product                       | Solar Gen | What Changed                                                   |
|------|-------------------------------|-----------|----------------------------------------------------------------|
| 2019 | Fenix 6 Solar                 | Gen 1     | First solar Garmin – dual-density panel, reddish tint           |
| 2020 | Instinct Solar                | Gen 1     | Solar enters the mid-tier lineup                               |
| 2022 | Instinct 2 Solar              | Gen 1     | Broader availability, same core architecture                   |
| 2024 | Fenix 8 Solar / Enduro 3      | Gen 2     | Bezel-only cells, no tint, Power Sapphire, 5× power            |
| 2025 | Instinct 3 Solar              | Gen 2     | Gen 2 solar trickles down, "unlimited" in multiple modes       |


## What’s actually under the glass?

Every Garmin solar watch is, at its core, a sandwich – a literal stack of layers, each with a specific job, bonded together in a precise order. Understanding this stack is the key to understanding why these watches behave the way they do in the real world.

### The Solar Sandwich

The structure, from the outside in, goes: cover lens → solar charging layer → display → motherboard. That's it. The solar component doesn't sit on top of the watch or clip on as an accessory – it's embedded between the glass you touch and the pixels you read. This is why you can't tell most Garmin solar watches have solar capability just by looking at them.

Where things get interesting is the cover lens itself. Garmin offers two variants:

**Power Glass** uses a standard strengthened glass lens with solar cells bonded to its underside. It's durable, optically clear, and shows up on mid-tier products like the Instinct series. It does the job well, but glass is glass – it scratches.

**Power Sapphire** replaces that glass with synthetic sapphire crystal. Sapphire is the second-hardest transparent material after diamond, rating 9 on the Mohs scale. That means it's essentially unscratchable in normal use. The solar component sits under this sapphire layer, fully protected. Power Sapphire shows up on the Fenix 8 Solar, Enduro 3, and tactix 8 Solar – the watches Garmin expects to get really field tested.

In the Gen 1 design (2019–2024), the solar panel was semitransparent and stretched across the entire watch face – both the bezel area and the main display. In the Gen 2 design (2024+), the solar cells moved entirely to the non-screen portions of the face, sitting in a dense ring around the display. This is why Gen 2 eliminated the reddish tint: there's simply no solar material between your eyes and the pixels anymore.

<!-- ![Gen 1 (2019–2024) on the left and Gen 2 (2024+) on the right comparison](/assets/images/garmin-glass.png) -->

<figure>
  <img src="/assets/images/garmin-glass.png" alt="Gen 1 (2019–2024) on the left and Gen 2 (2024+) on the right comparison">
  <figcaption>Gen 1 (2019–2024) on the left and Gen 2 (2024+) on the right comparison</figcaption>
</figure>

### Photovoltaic Efficiency and Panel Orientation

Not all parts of the solar layer are pulling their weight. In the Gen 1 setup, only the thin ring of solar cells around the edge of the display worked at close to full efficiency. Most of the main screen was covered by a semitransparent solar array that let about 90% of light pass through so you could actually see the watch face. That means only about 10% of the incoming light was even available for solar conversion. And when you factor in the limitations of the thin film tech, the actual energy conversion rate for the center of the screen drops below 1%, while perimeter cells do most of the charging with ~8-10% net effective efficiency.

But raw cell efficiency is only half the equation. The other half is geometry.

Solar panels are at their best when sunlight hits them straight on, at a perfect 90-degree angle to the surface. As soon as you tilt the panel, the output starts to fall off – sometimes fast. With a curved watch face, you’re already taking about a 20% hit compared to a flat panel, just because the light isn’t hitting every part evenly. That’s the easy part. The bigger challenge is how often a wearable is angled away from the sun. If the panel is facing sideways (90 degrees off from the sun), output doesn’t just dip – [it drops to about one-sixth of what you’d get at the optimal angle](https://the5krunner.com/2025/11/27/garmin-solar-technology-power-glass-unlimited-battery-mip/). And the reality is, your wrist is rarely pointed directly at the sun. Whether you're running, hiking, or just walking around, the angle of your watch keeps shifting, which means the amount of sunlight hitting the panel is all over the place from moment to moment.

Garmin’s engineers know this, so they don’t just rely on a single orientation for their solar panels. Instead, the solar array is made up of multiple tiny panels, each set at a slightly different angle. The idea isn’t to chase the perfect sunbeam all day, but to make sure the watch grabs at least some energy no matter how you move. That way, whether you’re checking your pace, grabbing a snack, or waving at a friend, your watch keeps soaking up what it can.

The entire array is managed by a Power Management Unit running **Maximum Power Point Tracking (MPPT)** algorithms. MPPT is a technique borrowed from rooftop and industrial solar installations: the controller continuously adjusts the electrical operating point of the solar cells to extract the maximum possible power at any given moment, accounting for changing light intensity, angle, and temperature. It's the same principle your home solar inverter uses.

Garmin's stated benchmark is [3 hours per day at 50,000 lux](https://garminkw.com/product/instinct-2x-solar-graphite/) – that's bright outdoor sunlight, roughly what you'd get on a clear day at mid-latitudes. For reference, full direct sun near the equator at altitude can hit 150,000 lux – 2x-3x the benchmark.

### The Real Numbers

Under favorable conditions–when the watch is left unused in direct sunlight throughout the day–Garmin solar technology can restore [approximately 20% of the battery charge](https://the5krunner.com/2025/11/27/garmin-solar-technology-power-glass-unlimited-battery-mip/). This is when using early Gen 1 equipment; Gen 2 improves this figure. However, the charging rate follows a diminishing-returns curve. As the battery charges, the rate at which it charges from solar energy decreases. Charging from 10% to 30% is noticeably faster than charging from 80% to 100%. This is standard lithium-ion charging behavior, but it means solar is most impactful when you need it most – when the battery is low.

For cycling products like the Edge 840 Solar and Edge 1040 Solar, Garmin quantifies solar contribution differently: as "ride gain." Roughly 20 extra minutes of battery life for every hour of riding in the sun. Whether Garmin expands this kind of solar quantification to future watch models still to be seen, but it's a smart way to make an invisible feature feel concrete.

---

## Inside the Watch – What the Teardowns Reveal

Spec sheets tell you what a watch does. Teardowns tell you how – and sometimes why. Cracking open three generations of Garmin hardware reveals a story of platform evolution, strategic component reuse, and some surprising decisions about what changes between price tiers and what doesn't.

### The Brain: From Cortex-M4 to Cortex-M33

The Fenix 6X Pro (2019) ran on an [NXP Kinetis MK28FN2M0](https://www8.garmin.com/manuals/webhelp/fenix66s6xpro/EN-GB/GUID-9DF0C26B-D40D-4CE9-801C-B8E6E1860F49.html) – a 150 MHz ARM Cortex-M4 with a single-precision floating point unit, 2 MB of flash, and 1 MB of SRAM. Garmin's firmware ran directly on the metal – no Android, no heavyweight OS – and the Cortex-M4's job was to be efficient, not fast. The 150 MHz top speed was a burst mode (NXP calls it HSRUN), used sparingly the way Intel uses Turbo Boost: hit peak clock for a brief computation, then drop back down. Normal operation likely sat well below that ceiling, with Garmin's firmware aggressively managing clock scaling to keep power draw minimal.

This same chip powered the Fenix 5 Plus series before it. Garmin got multiple product generations out of a single processor – a pattern that continues to this day.

Fast forward to the Fenix 8 (late 2024), and the brain has finally changed. The main SoC is now an [NXP i.MX RT500](https://www.nxp.com/products/i.MX-RT500) (MRT595SFF0C) – a dual-core processor pairing an ARM Cortex-M33 main core with a Cadence Xtensa Fusion F1 DSP, both capable of running up to 200 MHz. The Cortex-M33 is a generational step up from the M4: it adds TrustZone security extensions, improved branch prediction, and better power efficiency at equivalent workloads. The DSP core handles audio processing – relevant now that the Fenix 8 has a speaker and microphone.

Interestingly, the heart of this watch is the same silicon used in the Fenix 7X. While that might seem like a carry-over, it’s actually a standard in the industry. These ultra-efficient Cortex-M chips don’t follow the yearly upgrade cycle of a smartphone; instead, they focus on maximizing every drop of power. There truly isn't a better-suited chip on the market for this mix of performance and efficiency. The RT500 remains highly capable, [featuring 5 MB of on-board SRAM and a 2D graphics engine](https://www.zlgmcu.com/data/upload/file/Utilitymcu/IMXRT500EC.pdf). Its most impressive trick is granular power management – effectively shutting down unused memory blocks to save energy. It’s this level of optimization that allows for weeks of battery life rather than days.

Moving to 5 MB of on-chip SRAM marks a significant departure from the Fenix 6X Pro’s design. Older generations required external **W987D6HB** LPDDR RAM – like the 16 MB Winbond chip found in the 6X –primarily to buffer map data. With the Fenix 7 and 8, Garmin has eliminated that external chip entirely. Using the RT500’s internal memory instead removes a major power-drain and reduces latency, even if it means map rendering is visibly slower than on devices with external RAM. It’s a classic Garmin optimization: they’ve intentionally traded UI "snappiness" for the extreme battery endurance the series is known for.

### The Generational Comparison

| Component     | Fenix 6X Pro (2019)                                    | Fenix 8 Solar 51mm (2024)                        | What Changed                                      |
|---------------|--------------------------------------------------------|--------------------------------------------------|---------------------------------------------------|
| Main SoC      | NXP Kinetis K28F (Cortex-M4, 150 MHz)                  | NXP i.MX RT500 (Cortex-M33 + DSP, 200 MHz)       | Architecture upgrade, integrated DSP and GPU       |
| RAM           | 16 MB external LPDDR (Winbond)                         | 5 MB on-chip SRAM (no external)                  | Eliminated external chip; less total, more efficient |
| Storage       | 32 GB eMMC (Toshiba)                                   | 32 GB eMMC (Foresee)                             | Same capacity, different vendor                    |
| GNSS          | Sony CXD5603GF (single-band, 6 mW)                     | Synaptics SYN4778 (multi-band, 7nm)              | Major upgrade – dual-frequency, more constellations |
| BT/WiFi       | Cypress CYW20719 + Atmel ATWILC 1000B (separate)       | Silicon Labs RS9116-B00 (combined)               | Consolidated into single chip                      |
| PMIC          | Maxim MAX20303B                                        | Maxim MAX20360                                   | Updated, same lineage                              |
| Sensor Hub    | Ambiq Apollo 2 (Cortex-M4, 48 MHz)                     | Ambiq Apollo 3 (Cortex-M4F, 48–96 MHz)           | Incremental upgrade                                |
| Audio         | None                                                   | Cirrus Logic CS47L24 codec + MEMS mic + speaker  | Entirely new subsystem                             |
| Touch         | None (buttons only)                                    | Cypress/Infineon PSoC 4000S (Cortex-M0+)         | New dedicated touch controller                     |
| NFC           | NXP PN81T                                              | NXP 22T3XR                                       | Updated, same function                             |
| Battery       | 420 mAh, 3.8V, 1.596 Wh                                | 618 mAh, 3.91V, 2.42 Wh                          | 47% more capacity                                  |


While the 32 GB storage and Maxim PMIC remain unchanged, Garmin didn’t just sit on their hands. By moving the RAM directly onto the SoC and consolidating the wireless chips, they simplified the watch's "brain" to save the energy. The standout upgrade is the new 7nm Synaptics GNSS chip: switching from an older Sony single-band solution to a modern, multi-constellation, dual-frequency chip is arguably the most significant functional improvement we've seen between these two models.

### The Dirty Secret: Same Silicon Across Price Tiers

Now here's where it gets interesting for anyone shopping between a $350 Forerunner and a $1,000+ Fenix.

A teardown of the **Forerunner 245 Music** (a mid-tier watch from 2019, the same generation as the Fenix 6X Pro) revealed that the major internal components – including the ARM processors – were **99% identical** to the Fenix series. Same NXP Kinetis main CPU. Same core architecture. The differences came down to peripheral additions: NFC presence, sensor count, storage capacity, and crucially, software-enabled features (that’s the key selling point)

This means the platform-level efficiency gains – clock scaling, power management firmware, GNSS optimizations flow across the entire lineup. When Garmin's firmware team improves power management on the Fenix, those improvements benefit the Forerunner too, since they run on the same silicon.

There are two ways to read this. On one side: Garmin gates features in software to justify higher price tiers on effectively identical hardware. On the other hand, a shared development platform means cheaper watches get the same stability, bug fixes, and productivity improvements as the flagships – rather than being left on an orphaned codebase. Both readings are probably true simultaneously. But from a battery-power-efficiency standpoint, the implication is clear: the engineering investment Garmin makes at the top of the lineup compounds downward through the product line.

### The Battery Itself

The Fenix 6X Pro packed [a 420 mAh, 3.8V lithium-ion cell](https://www8.garmin.com/manuals/webhelp/fenix66s6xpro/EN-GB/GUID-9DF0C26B-D40D-4CE9-801C-B8E6E1860F49.html) (1.596 Wh) manufactured by Routejade Inc. – actually 10 mAh less than the Fenix 5X before it, yet lasting significantly longer thanks to the hardware and firmware optimizations described above.

The Fenix 8 51mm jumps [to 618 mAh at 3.91V](https://www8.garmin.com/manuals/webhelp/GUID-EECCAC99-90D6-4AB1-9A3A-EC433D3365E2/EN-US/fenix_8_Series_OM_EN-US.pdf) (2.42 Wh) – a 47% increase in raw capacity. Combined with the more efficient SoC, consolidated radio chips, and improved GNSS, this is how you get from the Fenix 6X Pro's 21-day smartwatch claim to the Fenix 8 Solar's 48-day-with-solar claim. 

One detail from the Fenix 6X Pro teardown worth noting: there was an **empty chip space** on the motherboard near the PMIC, positioned exactly where a solar charge management IC would sit. The non-solar Fenix 6X Pro was literally designed with a space for the solar variant's hardware.

![Fenix 8 Solar & Fenix 6X Pro teardown](/assets/images/teardown.png)

---

## MIP vs. AMOLED – The Display Tradeoff at the Heart of Battery Life

If you've made it this far into the internals, you've seen how Garmin squeezes efficiency out of every chip on the motherboard. But none of it matters as much as one decision that's visible every time you glance at your wrist: which display technology is behind the glass.

Garmin offers two types across its lineup, and the choice between them determines how long the watch lasts, how well solar charging works, and ultimately who the watch is built for.

### MIP: The Efficiency Play

Memory-in-Pixel (MIP) displays are transflective – they reflect ambient light back through the display to create a visible image. In direct sunlight, an MIP panel actually gets easier to read. No backlight is needed during the day, and the display draws power only when pixels change, not to maintain a static image. This is fundamentally different from how most screens work, and it's why MIP watches can idle for weeks.

The tradeoff is real: MIP panels look dated next to modern AMOLED screens. Lower contrast, muted colors, no deep blacks. The Enduro 3's MIP display is noticeably improved over the Enduro 2 – brighter, better viewing angles, and the solar ring is no longer a visible red bar at the perimeter – but put it next to a Fenix 8 AMOLED and the gap is obvious. Garmin is not unaware of this. They're deliberately choosing it, and the battery numbers explain why.

### AMOLED: The Experience Play

AMOLED panels are beautiful. Each pixel emits its own light – vivid colors, true blacks, sharp text, smooth animations. Garmin offers AMOLED variants across the Fenix 8, Instinct 3, and Venu lines, and they look like a completely different product category from their MIP siblings. But every one of those self-lit pixels draws power, constantly, for as long as the display is on.

| Model                   | Display      | Smartwatch Battery         | GPS Battery             |
|-------------------------|--------------|---------------------------|-------------------------|
| Fenix 8 51mm AMOLED     | AMOLED       | Up to 29 days             | Up to 84 hrs            |
| Fenix 8 51mm Solar      | MIP + Solar  | 30 days (48 w/ solar)     | 149 hrs (332 w/ solar)  |
| Instinct 3 50mm AMOLED  | AMOLED       | Up to 24 days             | Up to 28 hrs            |
| Instinct 3 50mm Solar   | MIP + Solar  | 40 days (∞ w/ solar)      | Unlimited w/ solar      |
| Enduro 3                | MIP + Solar  | 36 days (90 w/ solar)     | 120 hrs (320 w/ solar)  |


Look at the Instinct 3 row. Same watch generation, same chassis, same processor, same solar-or-not decision point – but the AMOLED variant tops out at 24 days and 28 hours of GPS. The MIP Solar variant hits unlimited in multiple modes. That's not a small delta. That's a different product category.

### Why MIP + Solar Is the Killer Combo

Garmin’s solar charging isn’t pumping tons of power into your watch – think tiny amounts, measured in milliwatts. But because an MIP display sips so little energy, even that trickle of solar has a meaningful dent. Low power use plus a steady solar top-up means you can actually break even – or even come out ahead. That’s how you end up with the little infinity symbol instead of a battery percentage.

On an AMOLED watch, the display's power hunger dwarfs the solar contribution. The solar panel would be feeding a furnace with a candle. This is why Garmin doesn't offer a solar AMOLED watch – and why every model that displays ∞ for battery life uses an MIP panel.

Garmin has filed **patents for solar AMOLED technology**, which suggests they're at least exploring the problem. But the physics is stubborn: AMOLED's power floor would need to drop by an order of magnitude, or solar efficiency would need a generational leap, before the combination makes practical sense. For now, it remains just a patent filing.

<figure>
  <img src="/assets/images/MIP_vs_OLED.png" alt="MIP (left) vs. OLED (right)">
  <figcaption>MIP (left) vs. OLED (right)</figcaption>
</figure>

---

## The Competitive Landscape and Where Things Are Heading

We've spent the last several sections inside Garmin's hardware. Now let's zoom out and ask the obvious question: is anyone else doing this, and where does the technology go from here?

### The Competition – or Lack of It

Garmin launched its first solar watch in 2019. Six years later, the competitive response has been remarkably thin.

Apple sits at the opposite end of the design philosophy. The Apple Watch Ultra 3 – Apple's most endurance-focused wearable at $800 – offers roughly 14 hours of GPS recording with full accuracy. That's a single long day in the mountains, maybe. Most Garmins deliver two, three, or four times that without solar, and models like the Enduro 3 push past 300 hours with it. Apple optimizes for a different set of priorities – cellular connectivity, a rich app ecosystem, deep iPhone integration – but if your use case involves being away from a charger for more than 36 hours, it's not a competitive option. The comparison isn't even close enough to require a table.

Coros is the more interesting competitor. They've built a strong reputation for lightweight, long-battery GPS watches at aggressive price points, and they actually shipped a solar product: the [DURA bike computer](https://www.wired.com/review/coros-dura-solar-gps-bike-computer/). On paper, the DURA's solar implementation rivaled or exceeded Garmin's Edge units. In practice, the product shipped with enough software and reliability issues that it hasn't translated into real competitive pressure – at least not yet. Critically, Coros still has no solar watch in its lineup. Their watches compete on efficiency alone, and they do it well, but they haven't attempted the solar integration that defines Garmin's leading products.

Most other wearable manufacturers – Samsung, Suunto, Polar – haven't shipped solar at all. Garmin's moat here isn't just the solar lens itself. It's the vertical integration: the Power Sapphire lens, the MIP display optimized for low power, the MPPT power management, the aggressive firmware clock scaling – all developed in-house and refined over six years and two hardware generations. Replicating any one piece is feasible. Replicating the stack is a multi-year engineering program.

### Why Solar Wins – and Why Nothing Else Is Close

If you're wondering whether alternative energy harvesting could eventually replace or supplement solar on wearables, the physics is blunt. Here's what each approach produces per square centimeter:

| Technology                | Power Output          |
|---------------------------|----------------------|
| Photovoltaic (solar)      | 1–10 mW/cm²          |
| Triboelectric (friction)  | 0.01–0.1 mW/cm²      |
| Piezoelectric (motion)    | < 0.01 mW/cm²        |
| Thermoelectric (body heat)| < 0.001 mW/cm²       |


On a device with a few square centimeters of harvestable surface, nothing else generates enough energy to meaningfully affect battery life. [Thermoelectric generators](https://www.nature.com/articles/s41598-022-23735-0) – the "charge your watch from body heat" concept that surfaces periodically in tech press – produce so little power on a wrist-sized device that they can't even offset the energy cost of reading the sensor that measures the temperature differential. Solar isn't just the best option for wearables right now – it's the only option that works.

### The Paradox of Efficiency

Here's where the long-term trajectory gets interesting, and slightly contradictory.

Every generation of hardware is getting more efficient on its own. The jump [from the Sony CXD5603GF GNSS chip (Fenix 6X) to the Synaptics SYN4778 (Fenix 8)](https://garminrumors.com/garmin-fenix-8-teardown-whats-actually-new-inside/) brought both multi-band capability and lower power consumption. MIP displays are getting brighter without drawing more power. Processors are consolidating functions – Bluetooth and WiFi merged into a single chip, external RAM eliminated – reducing total board power. The result: non-solar models now routinely deliver multi-week battery life that would have been flagship-tier a few years ago.

This creates a reasonable question: if the baseline is already this good, **does solar still matter**?

The answer is yes, but the reason flips. When overall power draw was high, solar was a modest supplement – a few extra hours here and there. Now that power draw is low, solar's contribution represents a much larger percentage of total consumption. A few milliwatts of solar input against a watch drawing 50 milliwatts is noise. The same input against a watch drawing 5 milliwatts is transformative. This is exactly how the Instinct 3 Solar reaches "unlimited" in modes where the Instinct 2 Solar couldn't – the Gen 2 solar hardware is better, but the lower-power platform is doing just as much of the work.

That said, solar doesn't make sense everywhere. Garmin's newer Edge bike computers use Transmissive LCD displays – large, power-hungry screens where solar gains are marginal relative to total consumption. Garmin quietly dropped the solar option for these models, which suggests the company is being rational rather than dogmatic about where solar adds genuine value.

### What to Watch For

Looking ahead, a few things have me really curious. Garmin’s work on solar AMOLED is easily the most intriguing – if they can make that work without killing the battery, it changes everything. More realistically, expect MIP screens to keep closing the gap; they’re getting so good that the outdoor legibility "advantage" over AMOLED is becoming a given. We’ll also see new GPS chipsets that sip power rather than gulping it, which is great for anyone doing long efforts. It’ll also be interesting to see if anyone else–looking at you, Coros – can actually build a solar watch that works. Garmin has a massive head start, but in tech, no lead is ever truly safe.

We might not know the exact timeline, but we know the direction, since every part of the watch is trending toward "ultra-low power," and solar efficiency keeps rising. We’re already at the point where these watches can live forever in low-power modes. Now, the challenge is seeing how much of the "smart" stuff we can keep running before the battery starts to dip.

## Who Actually Needs This?

After a ton of text on silicon, solar physics, and teardown photos, it's worth asking the practical question: should you actually pay the solar premium?

If you're planning multi-day expeditions, running ultras, bike touring across remote terrain, or simply the kind of person who forgets to charge things and resents being punished for it, solar with an MIP display is definitely a choice. You pay more upfront for the ability to stop thinking about battery life entirely. For these use cases, nothing else on the market comes close, and it's not a competitive gap that's closing anytime soon.

If you're a daily gym-goer who charges every night on the nightstand anyway, an AMOLED Garmin – or frankly any modern smartwatch – is perfectly fine. Solar is a useful extra at that point, not a necessity. The beautiful screen will matter more to you than an extra two weeks of theoretical runway you'll never use.

After all, the small infinity symbol on your wrist means that you are wearing something special. It is not just a clever invention, but the result of a whole series of engineering tricks, which you now know more about. That “∞” isn’t just a flex; it’s a reminder that every layer inside is doing its part to keep you going, no matter where you take it!


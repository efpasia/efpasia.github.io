---
layout: post
title: "Code Is a Commodity Now"
date: 2026-10-15 12:00:00 +0800
categories: thoughts
---

## The numbers first

On [Djinni](https://djinni.co/), Ukraine’s largest tech job board, frontend fell from 5.1% of all vacancies in January 2025 to 1.6% in August 2026 (DOU trends). Applications per frontend listing went from 59 in March to 112 in September. Developers as a whole went from 67% of all hires a decade ago to 36% now.

The easy objection is that this is one wartime market. Fine, but the same curve shows up in markets with no war in them. [Indeed Hiring Lab](https://www.hiringlab.org/)’s index of US software-development postings sits roughly 70% below its 2022 peak and about a quarter below pre-COVID levels. Of the past year’s modest rebound, 71% came from senior roles. The junior end is worse: a Stanford analysis of ADP payroll data found employment of US software developers aged 22 to 25 down about 20% since late 2022, while older engineers in the same roles grew. That’s the demand side. On the supply side, Sundar Pichai said in April that [75% of Google’s new code is AI-generated](https://www.semafor.com/article/04/24/2026/google-ceo-says-75-of-companys-new-code-is-ai-generated). It was 25% in 2024 and 50% last fall.

More and more prospects ask for teams of one to three people: one person who designs and ships the frontend, one who handles the backend, DevOps, and everything else. The classic setup with separate designers, frontend and backend developers is, as they put it, a legacy org chart. “That’s the default now,” they say.

If you came for the “AI replaces everyone” post, or the “AI hacks everything” one, this isn’t it. I have little patience for either camp. The closest economic rhyme I can find is the US railroad build-out: wildly overhyped in its moment, capex running far ahead of real revenue, priced almost entirely on projections of future profit. Many railroads went bust, and the economy still got rewired on the tracks they left behind. That’s roughly where AI is now. Megacap spending isn’t backed by current revenue, the promised GDP miracle is nowhere in the data yet, and the structural shift is happening anyway. The development market is changing shape rather than emptying out. But I’m getting sidetracked - that’s a topic for another post.

So this is the colder read: the cost of producing working software is deflating, and everything priced on top of that cost will reprice. I want to trace where the margin goes, because I think the direction is a lot more visible than most of the market admits.

---

## The mechanism

Commoditization follows a script old enough to trust, and open source is the version everyone in software has lived through. In the mid-1990s Netscape sold web servers for real money. Within a few years Apache was free, and Microsoft was giving IIS away with Windows, and the business of selling a web server had all but vanished. The money didn’t disappear, though. It moved to the companies that would run those servers for people who didn’t want to run them themselves, first hosting firms, then the cloud.

Code entered this script a while ago; what’s new is the speed. Google went from 25% of its new code being generated to 75% in about eighteen months. That is a cost curve, whatever Pichai meant it as. One of the largest engineering organizations on earth now ships the machine-written line three times out of four, so a line of working code costs Google a fraction of what it did in 2024, and what it costs Google is what it costs everyone a few years later. You can already see it on sales calls: the client shows up with a Lovable prototype that runs, and the quote has to explain why finishing it costs more than the subscription that produced it. “We build well, fast, and cheaper than before” stops being a pitch at that point. The client assumed it before they emailed.

---

## What this does to the services market

A service business sells intangible value: time, specialized knowledge, labor, convenience. It exists to relieve a pain the client can’t or won’t handle themselves. For twenty years, “we can build software” qualified, because building was scarce. So the industry priced the scarce part: dev hours.

That pricing basis is now deflating from three directions at once. Customer acquisition costs keep rising across outsourcing. Engagements run shorter because clients expect faster time-to-market. And the deliverable itself is sliding into the commodity column: when a client can get a product running on a $200/month subscription and a handful of SaaS platforms, there is no financial reason to pay a team for the build. No sales process fixes that, because the pain the whole business was built to relieve is going away.

A commoditizing service earns less margin per unit of itself, permanently. That leaves two strategies I’d take seriously:

**Volume:** keep selling default development, accept the shrinking margin, and win on predictably cheap customer acquisition at scale. This is a real business, just a different one, closer to hosting than consulting. Most agencies have neither the sales machine nor the cost structure for it, and they’d be up against large offshore development firms that have spent decades industrializing exactly this kind of work. Competing with them on price is like a small Western workshop trying to beat contract manufacturers on unit cost.

**Reposition:** move sideways into what a client cannot get from a subscription: knowing their domain better than they expected you to, telling them which half of the spec not to build, design a founder can’t get from a template, owning the outcome and eating the risk when it doesn’t land. Trust most of all. When anyone can generate code, someone still has to put their name under “this is correct, this is safe, this was worth building,” and that signature is the slowest thing in this industry to commoditize. The deliverable shifts from “the application” to “the decision and the result,” and pricing, as expected, follows it.

---

## The regulated-market trap

There’s a third option that looks pretty attractive at first: take shelter in regulated industries. Think MedTech, FinTech, InsurTech, RegTech, GovTech, PropTech, LegalTech—you know, all the "-Tech"s with compliance headaches. The logic makes sense: regulations slow things down, so the wave of commoditization reaches these sectors a little later. But that’s just it—later, not never. In my experience, these industries generally run about two years behind the wider market, and then the same trends catch up. Regulation (or governments, since GovTech is always slowest) might drag out the timeline, but it doesn’t change where things are headed. So sure, jumping into a regulated vertical could buy you some breathing room to rethink your game plan, but if you treat it as your main strategy, all you’re really doing is postponing the same challenge until 2028.

---

## Where I’d argue against myself

The productivity story isn’t clean, and I’d be cheating if I pretended otherwise. [METR](https://metr.org/) ran a randomized trial last year: experienced open-source developers using early-2025 tools were 19% slower in their own repos, and believed they were faster the whole time. Google’s DORA 2025 survey has over 80% of developers saying they feel more productive. I don’t think either side is lying. What I see fits the trial: on a greenfield project or a pile of boilerplate, the tools are absurdly good, and inside a dense ten-year-old codebase they produce plausible code that takes longer to check than it would have taken to write. But a good harness around the project changes the picture. [ClickHouse](https://clickhouse.com/blog/agentic-coding) gets wonderful results that way, and the tools keep evolving and getting more polished.

The other thing the doom reading misses is Jevons paradox: cheap inputs expand consumption. Every SMB that could never afford custom software, internal tools for teams of five, vertical micro-products in industries consultants ignore, software with exactly one user. The market for writing code deflates while the software market grows by an order of magnitude, both at the same time. That’s why I care less about whether there will be less work than about who captures the new shape of it.

---

## The trends to watch

**Role consolidation.** The “frontend developer” listing is turning into a “design engineer” or “product engineer” listing: one person with taste who ships, with agents doing the typing. [Vercel has a whole team under that title](https://vercel.com/blog/design-engineering-at-vercel), Cursor’s new Head of Design is hiring for it, and it keeps showing up in Anthropic, Stripe, and Lovable postings.

**Pricing migration.** From hours toward fixed, outcome, and value pricing. Billing fixed while delivering with AI is real margin today, but every competitor will be doing it within a couple of years, so it’s rather an arbitrage opportunity than a real strategy.

**Team compression.** The one-to-three-person team from the top of this post is where this ends up: small generalist teams running agents ship what took eight to ten people in 2023, so each project needs fewer people, and each person carries more projects. Indeed’s senior-heavy rebound fits that picture.

**The junior squeeze.** Newcomers feel it most. The entry bar was already high, with a crowded pool of fresh graduates competing for the same junior roles, and it’s rising further as most of the market sees little reason to pay for a junior’s ramp-up. The entry point probably won’t disappear so much as get renamed: fewer “junior developer” postings, more roles built around orchestrating agents, whatever title sticks.

**Verification as a market.** AI-code audit, security review, accountability. High margin while it’s a practice, and a feature once someone turns it into tooling (What is the prospect for this?).

**The spec displacing the codebase as the asset.** Code as build output that gets regenerated more and maintained less. This is the furthest out of the six and the hardest to put a date on, but I find it the hardest to argue against directionally.

---

## The knock-on effects

A few second- and third-order effects I’d expect, held with less confidence than the trends above.

**Build beats buy again.** For twenty years, the default advice was to buy because building was the expensive option, and now it flips. The long-tail SaaS dies because the customer’s IT team rebuilds it in a week, with exactly the fields they need and none of the ones they don’t. Corporate IT turns back into a software producer, the way the mainframe shops of the 80s were before packaged software turned it into a procurement department (btw I had some really nice old article about mainframe developers and they are also good addition and comparison for AI in development, I’ll attach it if I find). The first casualties are the products that are priced on lock-in rather than on anything you couldn’t rebuild. People are already vibe-coding Photoshop clones for their own use. A subscription whose main retention tool is the pain of canceling and migration costs (Adobe’s, say) is in more trouble than one sitting on data, a network, or a compliance stamp.

**Hiring signals go blind.** Every signal the industry uses to hire measures typing and language knowledge. GitHub contribution graphs, LeetCode, years of experience, the live-coding round: all of them test whether a person can produce working code under time pressure, and that is the thing that just became cheap. Nobody has a good test yet for what is now scarce. Can they catch the bug the agent didn’t? Can they run four tasks in parallel without losing their minds? Do they think in product, or do they wait for a detailed ticket? Can they tell a client “don’t build this” and be right? I don’t know what that interview looks like. Probably something closer to a design review than a whiteboard: like here is a messy repo, here is an agent, here is a vague ticket from a real client, show me what you ask before you start.

Put three things together: someone has to carry liability for generated code, seniors are getting scarce, and nobody can tell from a CV who is good. That combination is what produced CPAs and the PE license for engineers. I’d expect a licensed engineer’s sign-off to become a legal requirement in a few verticals first (medical devices, finance, anything a government buys), then spread.

**Drag-and-drop no-code dies.** No-code was a workaround for expensive code. Bubble, Adalo, the long tail of visual app builders: all of them let a non-programmer get software without paying a programmer, at the price of living inside someone else’s box. When the programmer costs $200 a month and speaks English, nobody needs the box. The visual tools that survive are the ones that were never really about avoiding code. The ones whose whole pitch was “you don’t need a developer” discover that the customer no longer needs them either.

**The technical co-founder stops being a requirement.** For fifteen years the first question an investor asked a non-technical founder was “who’s building it?” and that question is losing its teeth. A founder with a domain and a customer list can get to a working product without a CTO, which means seed money stops funding the build and starts funding distribution: sales, content, channels, whatever gets the thing in front of buyers. Therefore rounds get smaller, because there is less to build before revenue. So we get more companies (?) with less seeding money. Which is the services-market conclusion one layer up: the build is no longer where the money gets decided.

---

## Where it goes

I don’t know where this ends, and anyone selling certainty about 2030 is selling something. What the past does show is where the money tends to go. The railroads were overbuilt, and plenty of the companies that laid the track went under. The fortunes went to whoever used the rails to move something people needed: steel, cattle. The value went to the layer on top. My list of what stays scarce is judgment, distribution, accountability, and taste. If I had to bet, I’d bet on distribution and accountability, because they depend on other people (customers, courts, insurers) rather than on how good the models get. Judgment and taste are the two I’d watch for the same treatment code just got.

For the services market specifically, the question itself has already moved. It used to be “can you build it?” It’s becoming “what should exist, and who owns the result?” The companies repositioning around that question get to discover the new margin structure firsthand. The ones waiting for 2021 to come back are already visible in the numbers up top: they’re the declining line, and they’re shutting down.

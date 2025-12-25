---
layout: post
title: "The API Tooling Crisis: Why developers are abandoning Postman and its clones?"
date: 2025-12-24 23:00:00 +0800
categories: thoughts
---

I’ve been a Postman user for my entire developer career. I remember when it was just a simple Chrome extension that made ~~pen~~testing APIs slightly less painful. Those were simpler times. Today, I find myself joining the growing exodus of developers abandoning these tools because they [sell out](https://geohot.github.io/blog/jekyll/update/2021/04/20/sell-outs.html).

## The Great Betrayal: how our favorite tools turned against us

There’s a disappointing pattern growing in the API tooling space that reflects what we’ve seen with other developer tools: a promising product gains support and adoption, then corporate pressures force it into enshittification.

Postman, once the darling of developers everywhere, has become the poster child for this transformation. The breaking point for many was the removal of the Scratchpad (offline mode). After that, degraded performance due to bloating and cloud lock-in. Suddenly, we were forced to sync our work to Postman’s cloud to access basic functionality (wasup Microsoft?). First of all, it’s spokes in wheels for the developers working on sensitive projects in banking, healthcare, or government sectors. I mean, that’s the point - to make them pay for the tool, to maximize shareholder value, and collateral damage dealt to the industry is not taken into account.

I tried to work around it. I attempted to use their “Lightweight API Client” which turned out to be a crippled version that barely functioned. Eventually, I gave up and started looking elsewhere.

Insomnia, which had positioned itself as the clean alternative to Postman, followed the same path, being architected as a copy of Postman. Version 8.0 locked users out of their local collections behind a mandatory login screen. The community backlash was [visible and fierce](https://github.com/ArchGPT/insomnium), but the fork was [abandoned](https://modernorange.io/item/41128221) and probably not used by anyone, while the original product became "another one corporate asset".

Thunder Client, the VS Code extension that promised simplicity, executed what can only be described as a [bait-and-switch](https://medium.com/@evanwilson9463/it-s-time-to-ditch-the-thunder-client-vscode-extension-6f6ee9793f46). After developers integrated it into their workflows, they moved the ability to save requests to local Git repositories behind a paywall. This wasn't just a business decision – it was just one more betrayal of the developer community that had helped them grow. And working on VS Code extensions is a pain in the ass.

## The Performance Crisis: why these tools feel like running through mud

Setting aside the ethical debates, performance is the main massive practical issue. The promise of Electron’s “write once, run anywhere” has turned into “runs poorly everywhere”. Postman takes 10+ seconds to launch and load on my MacBook M1 Pro. It eats gigabytes of RAM just to send a simple GET request, and it becomes [worse](https://community.postman.com/t/unusable-high-cpu-usage-is-anyone-listening-postman-version-7-18-1/10921) [over](https://github.com/postmanlabs/postman-app-support/issues/11976) the [years](https://community.postman.com/t/postman-application-extremely-slow-or-unresponsive-after-recent-update-10-13-5/47042). That’s horrible for something that is even simpler than an IDE.

The bloat isn't just in the RAM; it’s in the UI. Postman has mutated into a platform trying to be everything at once – an API repo, a social network, a mock server, testing framework. [Ninety percent of developers](https://stateofapis.com/2021/section/testing) just want to ping an endpoint and see the JSON. Instead, we’re dodging pop-ups and menus designed for enterprise sales teams and then just being locked out behind sign-up wall or paywall.

## Evaluating modern alternatives

This friction brought the community toward tools that value simplicity and, more importantly – Git workflows. Bruno is the name coming up most often. It’s open source and stores collections as plain-text files on your drive, which means I can version-control my API tests right alongside the source code in the same repo (unbelievable, right?)

It’s far from perfect, Bruno’s UI lacks polish, and migrating is a headache – scripts often break because the JavaScript sandbox isn’t the same as Postman’s. It also lacks some heavy-hitting enterprise features, such as advanced proxy support. But it's promising to respect you – the user. [At least for now.](https://biggo.com/news/202502011312_api-client-wars-lightweight-alternatives)

## What the Ideal API Tool Would Look Like

* Local-first, file-system centric. Collections and requests should be stored directly in the project repository.
* Import support for OpenAPI specs and GraphQL schemas, and straightforward test support.
* Zero login wall. The tool functions completely offline without forced account creation or mandatory cloud synchronization.
* Git-Native Collaboration. Collaboration happens through standard VCSs rather than paid “seat” licenses or proprietary cloud syncs.
* Native Performance. Built with high-performance languages (e.g., Rust) rather than web wrappers. Open instantly, consume minimal resources.
* Extensible Design. A modular plugin architecture that adds functionality and not bloating the core application.
* Universal Imports. Native support for importing OpenAPI specs, GraphQL schemas, Postman collections.
* Proxy Agnosticism. Tool must be designed to proxy traffic through any interception tool. Proxy-aware or browser-based architecture is must have.
* Scripting & Auth flows. Pre-request& post-response hooks.
* Straightforward testing. Built-in support for writing and running tests against API responses, by code.

## And what's next?

API tooling today sits somewhere between bloated, cloud-locked giants and lightweight, open, local-first newcomers. For every Postman or Insomnia, there’s a Bruno, Hurl, or Httpie — each being different points on the spectrum, but no single tool nails everything. Some are too barebones, some still need polish, and everywhere I look, it feels like there’s a missing piece.

But maybe that’s good. Maybe we’re still waiting for someone to finally break the compromise. I hope someone is working on it. Maybe it’s you. Maybe it'll be me. The best tool probably hasn’t been built yet.
---
layout: post
title: "Controlling cache TTL in CloudFlare worker"
tags: [CloudFlare, Caching, CloudFlare Workers]
author: Andreas Bjørn Hassing
---

I've been playing around with CloudFlare workers and pages to host a homebrewn app in front of our local washing house
app data. I wasn't a big fan of the existing one so I set out to bake one that worked better for me.

(Result: <https://vask.zenbit.dk>)

When playing around with caching, because I don't want to be a burden to the official backend, I was trying to understand
how I could utilize [workers caching API](https://developers.cloudflare.com/workers/runtime-apis/cache/) in that effort.

The response from the official backend has a `Cache-Control` header set to `private`, meaning that clients should decide
themselves how to cache results. Browsers will generally not cache results with that header key value, CloudFlare adopted
the same behavior.

To be continued...

---
layout: post
title: "Add lines to OneNote with Alexa"
tags: [OneNote, Alexa, Python]
author: Andreas Bjørn Hassing
---

My wife recently gave birth to our first child; a baby girl. A few days in, after we came back from the hospital, we realized that we kept forgetting at what time we had put her to sleep, and also which breast she was last fed from 🙄.

"Hmm 🤔", I thought. How can we solve this problem in the most ergonomic way possible?

[IFTTT](https://ifttt.com/) has a connector to [Google Sheets](https://ifttt.com/google_sheets), so we could create a spreadsheet and append rows to it via Alexa. Well, that is actually super helpful and would have been the end of this blog post, were it not for us already having created a OneNote section just for our new baby. I obviously wanted the rows to go into a OneNote page of my choosing in that section, to keep the data about her nice and cohesive.

Does IFTTT have a connector for OneNote? [Yes!](https://ifttt.com/onenote) Awesome! Oh.. It only supports creating new pages. Bummer.

How do I do this, then?

Entering: [Microsoft Graph](https://developer.microsoft.com/en-us/graph) <sup>([docs](https://docs.microsoft.com/en-us/graph/overview))</sup>!

You can play around with Microsoft Graph using the [Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer). It's great for prototyping and then moving those prototypes over to your project once it works.

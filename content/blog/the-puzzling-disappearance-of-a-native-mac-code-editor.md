---
title: "The puzzling disappearance of a native Mac code editor"
date: "2023-04-18"
description: "Zas Editor, a proclaimed high performant, native code editor for macOS mysteriously vanished. What happened?"
tags:
- "macOS"
- "miscellaneous"
---

A new code editor has been making waves recently — [Zed](https://zed.dev/). Zed describes itself as a, "high-performance, multiplayer code editor from the creators of Atom and Tree-sitter."

The name Zed, however, reminded me of another code editor that I remembered by its domain: zeditor.app. Upon revisiting the domain, I was made aware the site no longer exists.

I tried searching for the site, but there were no results other than a [Y Combinator post](https://news.ycombinator.com/item?id=30952084). From there, I was off to the Internet Archive.

The most recent capture of the site, [March 1, 2023](https://web.archive.org/web/20230301033702/https://www.zeditor.app/), features a `DEPLOYMENT_NOT_FOUND` Vercel error. It is on the [April 28, 2022](https://web.archive.org/web/20220428110453/https://www.zeditor.app/) capture that the site's latest available information is accessible.

Zas Editor (zeditor.app) proposes itself as essentially a Mac native editor with a fast Go and Rust backend and an Xcode-like frontend.

On paper, both Zed and Zas editor look similar: the use of Rust, the claims of high performance, and the initial Mac focus. However, I don't think there's any correlation; Zas Editor didn't silently transform into Zed.

So, why did Zas Editor disappear? I don't know 🫠

Zas Editor was an actual, functional product. Their [issue tracker](https://github.com/ZasEditor/Zas-Editor-Issues/issues) on GitHub shows that they had actual customers who were reporting issues, which is expected of any app that just launched. And this [screenshot](https://pond.lx-is.lu/static/080W4ELWUWKaKHk5CLNc.webp) by Discord user Tweety#6125 shows it running.

What I failed to mention previously was that Zas Editor was a premium code editor, costing $25. It very well could've been a cash grab, but it would be an odd one. Niche and high effort; relatively small target audience.

Malware... probably not? According to [ack3rd on GitHub](https://github.com/ZasEditor/Zas-Editor-Issues/issues/65#issuecomment-1127460149), there weren't any concerning outbound connections. And if infecting the most amount of people, a free model would theoretically make more sense.

The claimed lead developer of Zas Editor made an appearance in the CodeEdit Discord. [CodeEdit](https://www.codeedit.app/) itself is an in-development, open-source effort to develop a Mac native code editor.

In the Discord, "Ali Nili" introduces themselves:

> "Hi everyone, I'm the lead developer of Zas Editor, which is also a Native Mac code editor. I'm amazed by the community you created and the progress you made so far. Zas Editor is not open source, but I would love to discuss technical details with you and get your feedback! Let me know what you think."

Discussions did not meaningfully progress.

As I aforementioned, I have no idea why this project vanished. While it would be interesting to develop a solid conclusion, I have no intentions of digging deeper into the mystery.


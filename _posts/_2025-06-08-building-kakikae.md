---
layout: post
title: "Building Kakikae: A Time-Traveling Photojournalism Game"
date: 2025-06-08
categories: [devlog, game-dev, AI]
tags: [kakikae, gamedev, love2d, AI, time-travel]
excerpt: "Kakikae is a game about rewriting history with your camera and AI. Here's why I'm building it and how it's going so far."
---

> *Rewrite the past. Reshape the future.*

## What is *Kakikae*?

*Kakikae* (書き換え) is a game I'm building with [Love2D](https://love2d.org/) where you travel through time, take photos of historical moments, and use AI to rewrite what actually happened.

Each photo becomes a new historical record, and the world adapts accordingly — characters change, alliances shift, and timelines branch.

It's my attempt to blend:
- AI storytelling  
- Emergent simulation  
- Minimalist interaction (just a camera)  
- And some subtle chaos

---

## Why I'm Making This

I wanted to learn more about **how AI can shape gameplay** beyond NPC chat or content generation. Could AI be used *as the game mechanic itself*?

I also love history, speculative fiction, and small games with strange mechanics — so this idea clicked:

> What if your photos rewrote history?

Plus, I wanted a focused, 3-month solo project that would:
- Let me explore Lua/LÖVE in depth
- Experiment with AI prompting inside a game loop
- Build something weird and (hopefully) beautiful

---

## The Core Loop (so far)

Here’s the base mechanic I’m working with:

1. **Drop into a time period**
2. **AI gives you a list of key historical figures** from that era
3. **You explore and take a photo**
4. **AI generates a rewritten historical record** based on that photo
5. **The world reacts and shifts**
6. **You continue, watching history bend**

---

## Progress & Next Steps

✅ **Set up**
- Basic top-down camera and sprite system  
- Procedural crowd movement  
- Photo capture + screenshot cropping

➡️ **In Progress:**
- AI prompt tuning for character generation  
- In-game “news” UI based on AI output

➡️ **Next Up:**
- State system for NPC reactions  
- First playable historical scenario (probably Edo Japan or 1920s Paris)

---

## Want to Follow Along?

I’ll be posting weekly updates right here on **[teruo.dev](https://teruo.dev)**.  
I’ll cover:
- AI prompt design and mistakes  
- Game design decisions  
- Art direction and scope control  
- Code stuff (for devs like me)

If you’re curious about how a game like this comes together — or want to see if AI can actually tell a decent historical lie — stay tuned.

> 📰 First headline coming soon.


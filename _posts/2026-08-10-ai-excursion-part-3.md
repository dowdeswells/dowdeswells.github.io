---
layout: post
title: "SKILL: Layered design for separation of concerns"
date: 2026-08-10
series: "With AI For AI"
series_order: 3
---

## What's it all about
I start with the end goal in mind. What i really need in the design phase is a clear set of interfaces that allow the parts of the software to be able to be iterated upon in isolation. A simple expression of this is:

```
agent layer  ──▶  business layer  ──▶  storage layer
 (translates)      (domain logic)      (persistence, injected)
```

Importantly the business layer does not depend on anything. For what it needs, it provides interfaces for the other layers.

### ToDo - security, testing, refactoring
At this point the general code architecture is addressed but specific cross cutting concerns are a TODO. Specifically, looking at the source code I can see repeated code. I will address this as a priority and then move on to security hardening and iterating over the various testing phases. 

I also don't like the Microsoft AIAgent being exposed in the interface of the Agent layer. It ties to the implementation as Microsoft Agent Framework.

Testing has been touched on in this current version but i want a clear progression of test scopes including the ability to run every test in a docker compose - something a ci pipeline would possibly include before a deploy except I fear this may be a bit too much for most runners due to the need to startup a local SLM.


The full document, at the time of writing looks like: 

{% include collapsible-section.html 
   file='snippets/soc.md' 
   title='separation of concerns' %}

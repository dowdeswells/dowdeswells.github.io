---
layout: post
title: "Structured coding and refactoring"
date: 2026-08-15
series: "With AI For AI"
series_order: 5
---

## The goal
The separation of concern skill seems to be working quite well. The code does seem to be structured in a way that I understand and I'm reasonably happy with the quality. There are, however, quite a few occurrences of repeated code. Also the size of the classes can get quite large. I wonder whether a skill could find any improvements?

I'm going to work on this with the assumption that the AI knows all the concepts. It just needs me to tell it my preferences and priorities.

My first attempt at skill is a very simple one. Focusing on modular design, cohesion and coupling. So as not to make the skill too long I will throw in there a reference to Robert Martin's Clean Code.

## The first skill
Here is my first attempt at the skill.

{% include collapsible-section.html 
   file='snippets/structured-coding.md' 
   title='structured coding skill' %}

### The outcome
Running the skill via the agent on just a single class the CosmosEventStore produced the desired effect. The repeated code was removed, and some other changes were made, but nothing outside of the class, which is what I specified in the prompt.

## Gang of Four
I'm interested to know what I can get away with without writing too many words in the skill. I prompted the ai to look at the Cosmos event store and report on any Gang Of Four patterns that would go well with that code. The AI didn't need any more explanation. It went through the Gang Of Four patterns individually and came up with a report. In the end the recommendation was a strategy and a builder. Unfortunately it then implemented them as internal sealed classes inside the main class, which only made the file bigger. I guess a skill somehow needs to apply judgement on file size and general reusability of utilities.
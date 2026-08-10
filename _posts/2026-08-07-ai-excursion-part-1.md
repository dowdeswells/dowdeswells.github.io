---
layout: post
title: "Developing with AI, developing for AI"
date: 2026-08-07
series: "With AI For AI"
series_order: 1
---

## Goal: Agentic
I've decided to develop an application hosted on Azure. Developed, or should I say, co-developed with an AI agent and featuring some form of agentic processing.

The application is an expenses tracker. Something which has become slightly more pertinent given recent events.

**My goal is to develop the application, and also to develop the processes via which I will employ the AI agent to help me code.**

## Coding Agent Harness: pi.dev
At work I was using Copilot installed into Rider for .NET development.

Reviewing the outcome and upon researching what is available, it has become apparent that a more simple tool is needed. Less clutter. Something in a terminal, something full screen. Focusing on the task in hand - agent work vs output review.

This is provided by [pi.dev](https://pi.dev){:target="_blank"}

## Coding Agent: DeepSeek
pi.dev integrates simply with DeepSeek, just an environment variable with the DeepSeek API key
It seems pretty good so let's start there.

## What have i got so far

[The deployed Expense Tracker](https://expensetracker-app.thankfulbush-f2983888.australiaeast.azurecontainerapps.io/){:target="_blank"}

I've been working on the code for a week. At this point I have a working expense tracker that runs an agentic loop with a register set of tools (mcp) that Add and edit expenses and income. The storage is implemented in a CosmosDB with an Event Sourcing design (which I will talk about later) which makes perfect sense for an auditable transaction record. There is a Github Action to deploy to an Azure Container App. OAuth users are provided by an Auth0 IDP.

### AI Agent Costs
DeepSeek is really cheap compared to the others. I'm impressed. My total spend over this period is about $1.30. I did try Kimi K3 for one small task - adding keycloak as the dev IDP in docker compose. After $2.50 it still hadn't worked through the problems so I stopped pi.dev (simply type in a new prompt to "stop what you are doing") and changed back to DeepSeek which proceeded to finish it off with a tiny cost.

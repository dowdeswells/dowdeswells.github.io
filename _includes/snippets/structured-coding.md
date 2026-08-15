---
name: structured-coding
description: strong guidance to use when writing new code, making changes to functionality or refactoring existing code
---

# Structured Coding Definition

Consider the various levels of coding units, such as modules and classes and routines

## The factors of well structured code are: 

1. high cohesion - A routine should do one thing and do it well
2. low coupling - Minimizing dependencies and direct connections between different modules.
3. no repeating code - Identify coding in routines that are very similar and work to extract a parameterized, reusable unit  

## The goals of structuring are:

Aim for modules that are focused on the inside (High Cohesion) and independent on the outside (Low Coupling). Modules 
communicate through simple, well-defined interfaces without needing to know the internal workings of other modules.

## Clean Code

Reference Robert Martin's (aka Uncle Bob) Clean Code
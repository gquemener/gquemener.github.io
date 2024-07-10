---
layout: post
title:  "[Review] DDD - Service Boundaries (Adam Ralph)"
date:   2024-07-09 09:55:09 +0200
categories: ddd
---

_Reading time: 5 minutes_

## Introduction

Last month, [Tech Excellence](https://www.techexcellence.io/) ("A community of software developers and engineering leaders who are raising the bar of technical excellence across the world.") invited Adam Ralph to talk about Domain Driven Design and Service Boundaries.

You can find a link to a recording [here](https://www.youtube.com/watch?v=I5fhtBQ2wQU).

This topic is of special interest to me, as I have very often encountered cases where such boundaries were either missing or wrongly shaped, leading to hard to maintain softwares.

Here are my notes of his presentation, along with some references that I find connected to what he talks about.

## Coupling

  - Designing a big ball of mud (BBOM) is rarely done on purpose.
  - It is the result of small couplings (as small as a method call!) adding up over time.
  - Service Oriented Architecture (SOA) is about grouping concerns into autonomous, loosely coupled, runtime => Temporal coupling (for example, by making a synchronous http request from service A to B) is always around the corner, and will lead to a distributed BBOM !
  - (Side note) This effect is what's called [Software Entropy](https://en.wikipedia.org/wiki/Software_rot).
  - Coupling can be measured by the degree of likeliness that one component of a system will change when another one does.
  - Event integration is NOT a guarantee for loose coupling (Wrong boundaries will lead to ever growing events #BigFatEvent).

## BigFatEvent vs SlimEvent : What if events only carry an ID?

  - Problem: How does downstream actors know what the ID relates to?
  - Solution: Start the workflow by generating the ID, then send relevant information to all the actors using the same ID. Integration events can now only carry the ID to notify about advancement.
  - Bottom line is that information should not be cascaded/forwarded from one frontal service to subsequent services, otherwise this frontal service(s) will need to change when new requirements arise (which mean strong coupling between them!). All services should be provided with the relevant information during the workflow (Shipping, here is the recepient address. Finance, here is the payment information. Sales, here's the order details. etc.) and not when the final stage of the workflow is reached (PlaceOrder). That way, when a new payment mode needs to be supported, only the payment service needs to change.
  - "The coupling is gone".
  - (Side note) Check Adam's [example](https://www.youtube.com/watch?v=I5fhtBQ2wQU&t=817s) about the order checkout process, it's very explicit!

## Coupling is not necessarily a bad thing

  - "If you have three services with zero coupling, you don't actually have one system. You have three systems." [Ref](https://youtu.be/I5fhtBQ2wQU?t=1541)
  - The UI is a natural coupling point.
  - Loose coupling is achievable in the UI as well. Different part of the UI (of the same page) can be owned by different teams.
  - Check [that link](https://go.particular.net/tech-excellence-ui) to know more.

## Designing a service

  - "A service is the technical authority for a specific business capability."
  - (Side note) __Business capability__ is key here. It is very often mentioned on books related to Domain Driven Design as well.
  - (Side note) __Event Storming__ helps to find boundaries by highlighting pivotal events (example: once an order is placed, there's no going back, apart from starting another process: cancelling an order)
  - That means, it's not a single function, it's not a class, it's not a database, etc.
  - Heuristic: look for a miscellaneous place where left over is left (for example, a "common" or "shared" directory)! The questions that are in this spot will help shaping correct boundaries!
  - "If you're sharing anything more than an ID, that's a strong sign that your service isn't properly encapsulating."
  - Vertical Slice Architecture
  - Anti-requirement technic: use absurd questions to detect things that have no relation between them (example : a user name and status, designed in the same User class). Find piles of related things!
  - Modeling is not about the data model (although most of the developer's brain is wired to focus on it), it's about the behaviors, the scenarios that your model needs to support.
  - Look for big fat arrows between services!
  - Once the business says "that's obvious" to your service boundaries proposition, you know you're on the right track.

## Conclusion

Adam provides valuable insights about how we might ultimately end up designing a big ball of mud system. He talks with ease, clarity and with great examples about how easy it is to create strong coupling between bits of the system.

He also talks about a service-oriented alternative, where bits share nothing more than an identifier, which is the lightest form of coupling apparently!

He mentions using the UI to compose communication between the services (TODO: have a look at the link he mentioned, I suspect it is about micro-frontend architecture).

Finally, in 90 minutes, Adam achieves to highlight the consequences of designing wrong boundaries (or no boundaries at all), with clear illustrations. He also manages to point key characteristics that should be searched while designing boundaries (loose coupling, business agreement, vertical slices, team autonomy, ...).

To go further, [a free, online, extract of 5 days course about service boundaries](https://go.particular.net/tech-excellence-boundaries) is available.

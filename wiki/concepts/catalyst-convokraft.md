---
title: Catalyst ConvoKraft
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [catalyst-general-topics.md]
tags: [catalyst, convokraft, chatbot, ai, conversational]
---

# Catalyst ConvoKraft

## Definition

Catalyst ConvoKraft is an AI-powered service for building text-based conversational bots that can be embedded into Catalyst solutions. Bots interact with application users — responding to queries, performing actions, or fetching data. [Source: catalyst-general-topics.md]

## Key Aspects

### Components
- **Bots** — Conversational assistants embeddable in web applications
- **Actions** — Programmed tasks with business logic that bots perform during user interactions
- **Handlers** — Functions handling greetings, failures, and exceptions during interactions (e.g., welcome message handler)
- **Bot Operations** — Training, testing, and deploying bots to production

### Developer Tools
- Client JS SDK for ConvoKraft (for embedding in web apps)
- Catalyst CLI support for ConvoKraft development
- Server-side bot logic via Java, Node.js, or Python SDKs
- Handler functions can use any supported runtime

### Workflow
1. Create a bot in ConvoKraft console
2. Define actions with business logic
3. Configure handlers for edge cases
4. Train the bot
5. Test the bot
6. Deploy to production

## Sources

- [[catalyst-general-topics]]

## Related Concepts

- [[zia-services]] — Another AI service in Catalyst (ML/vision vs conversational AI)
- [[serverless-functions]] — Handler functions are implemented as serverless functions
- [[catalyst-console]] — Bot creation and management via console

[Source: catalyst-general-topics.md]

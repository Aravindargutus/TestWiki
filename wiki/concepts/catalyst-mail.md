---
title: Catalyst Mail
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, mail, email, smtp]
---

# Catalyst Mail

## Definition

Catalyst Mail enables sending emails from Catalyst applications. Emails are sent through configured domains and email addresses set up in the Catalyst console. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [Mail Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/mail/) — Introduction, Email Configuration, SMTP Configuration, Domains, Send Emails.

### Purpose
Cloud Scale component for securely sending transactional or mass communication emails to end-users. Supports per-department sender addresses (marketing / support / finance / etc.) configured directly in the console.

### Configuration Model (Console)
Three setup areas under **Mail** in the console:
| Area | Purpose |
|---|---|
| **Email Configuration** | Add / verify / manage sender email addresses; store multiple per project |
| **SMTP Configuration** | Optional — integrate an **external email client** via custom SMTP instead of Catalyst's built-in client |
| **Domains** | Configure public-domain addresses (no SMTP config needed for supported providers) or **private / organization-owned domains** |

### Client Options
- **Built-in email client** (default) — managed delivery optimized by Catalyst
- **Custom SMTP** — bring-your-own email client via SMTP settings

### Supported Features (Platform-Level)
- File attachments
- CC / BCC
- Public domain or private/custom domain as sender
- Multiple sender addresses per project
- High scalability under load

### Access Paths
- Java, Node.js, Python SDKs
- REST API (`SendEmail`)
- Console test-send (via sender config)

## Key Aspects

- **1 SDK operation**: `ZCMail.getInstance().sendMail(ZCMailContent)`
- **Limits per operation**:
  - To: 10 recipients
  - CC: 10
  - BCC: 5
  - Reply-to: 5
  - Attachments: 5 files, max 15MB total
- **Prerequisites**: Domain and email address must be configured in Catalyst console
- **Content**: Supports subject, body content, from address, to/cc/bcc lists

## SDK Entry Point

```java
ZCMailContent mail = ZCMailContent.getInstance();
mail.setFromEmail("p.boyle@zylker.com");
mail.setToEmailList(Arrays.asList("user@example.com"));
mail.setSubject("Subject");
mail.setContent("Body text");
ZCMail.getInstance().sendMail(mail);
```

## Node.js SDK Access Pattern

```js
const email = app.email();
await email.sendMail({
  from_email: 'noreply@example.com',
  to_email: ['user@example.com'],
  subject: 'Test Email',
  content: '<p>Hello!</p>'
});
```

Supports public domains, organization domains, and external SMTP clients. [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

**Key difference**: Java uses `ZCMailContent` bean with setters; Node.js uses a plain JSON config.

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js Mail (2 pages)

## Related Concepts

- [[push-notifications]] — Alternative notification channel (web/mobile push vs email)
- [[catalyst-authentication]] — Password reset uses email but via auth component, not Mail

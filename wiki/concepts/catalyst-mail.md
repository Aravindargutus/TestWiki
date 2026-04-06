---
title: Catalyst Mail
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, mail, email]
---

# Catalyst Mail

## Definition

Catalyst Mail enables sending emails from Catalyst applications. Emails are sent through configured domains and email addresses set up in the Catalyst console. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

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

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]

## Related Concepts

- [[push-notifications]] — Alternative notification channel (web/mobile push vs email)
- [[catalyst-authentication]] — Password reset uses email but via auth component, not Mail

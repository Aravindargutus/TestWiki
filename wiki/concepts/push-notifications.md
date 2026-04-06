---
title: Push Notifications
type: concept
created: 2026-04-06
updated: 2026-04-06
sources: [catalyst-java-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, notifications, push, mobile, web]
---

# Push Notifications

## Definition

Push Notifications in Catalyst supports sending notifications to both web and mobile (Android/iOS) applications. Web and mobile use separate SDK classes and have different requirements. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Key Aspects

- **2 SDK operations**: Web notifications, Mobile notifications (Android + iOS)
- **Web notifications** (`ZCWebNotification`):
  - Up to 50 users per call
  - Can target by user ID (Long[]) or email (String[])
  - Message: plain text, HTML, or JSON
- **Mobile notifications** (`ZCMobileNotification`):
  - Separate methods for Android and iOS
  - Requires registered app with `appID`
  - Must use [[catalyst-authentication]] for user identification
  - Supports badge count, message payload
  - Target by email address

## SDK Entry Point

```java
// Web:
ZCWebNotification.getInstance().notifyUser("Hello!", userIdArray);

// Mobile (Android):
ZCMobileNotification mobile = ZCMobileNotification.getInstance(appId);
mobile.sendAndroidPushNotification(new ZCPush() {{ setMessage("Hello!"); }}, "user@example.com");

// Mobile (iOS):
mobile.sendIOSPushNotification(push, "user@example.com");
```

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]

## Related Concepts

- [[catalyst-mail]] — Email as alternative notification channel
- [[catalyst-authentication]] — Required for mobile push (user must be authenticated)

## Evolution

- Web notifications came first; mobile notifications added later with separate class hierarchy.

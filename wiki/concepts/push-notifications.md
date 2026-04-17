---
title: Push Notifications
type: concept
created: 2026-04-06
updated: 2026-04-17
sources: [catalyst-java-sdk-cloud-scale-remaining.md, catalyst-nodejs-sdk-cloud-scale-remaining.md]
tags: [cloud-scale, notifications, push, mobile, web, ios, android, apns, fcm]
---

# Push Notifications

## Definition

Push Notifications in Catalyst supports sending notifications to both web and mobile (Android/iOS) applications. Web and mobile use separate SDK classes and have different requirements. [Source: catalyst-java-sdk-cloud-scale-remaining.md]

## Platform Overview

> Source: [Push Notifications Help](https://docs.catalyst.zoho.com/en/cloud-scale/help/push-notifications/) — Introduction, Architecture, iOS / Android / Web.

### Purpose
Cloud Scale component delivering remote notifications to users even when an app is not running. Used for promotional messages, alerts, offers, event updates, and actionable engagement prompts.

### Supported Platforms & Delivery Mechanism
| Platform | Delivery | Setup |
|---|---|---|
| **Web** | Catalyst Web SDK | Embed SDK in web client; register client via `register-client` |
| **iOS** | **APNs** (Apple Push Notification service) | Generate APNs certificate from Apple → upload to Catalyst; iOS/Flutter SDK registers device |
| **Android** | **Firebase Cloud Messaging (FCM)** | Generate FCM config from Firebase → configure in Catalyst; Android/Flutter SDK registers device |

### Client vs Server SDKs
- **Client-side SDKs** (register / de-register devices): iOS, Android, Flutter, Web
- **Server-side SDKs** (send notifications): Java, Node.js, Python
- **REST API** for all 3 platforms (Web / iOS / Android)

### Console Test Send
Test push notifications can be sent from the console to select devices for web, iOS, and Android to verify setup.

### Typical Flow
1. Configure APNs cert (iOS) / FCM project (Android) in Catalyst console
2. Client SDK registers device (linked to authenticated user)
3. Server SDK/API triggers `send-notifications` with target users
4. Catalyst forwards via APNs / FCM / Web push

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

## Node.js SDK Access Pattern

```js
// Web
await app.pushNotification().sendWebNotification({
  message: 'Test!',
  recipients: [{ recipient_id: userId, target_url: 'https://app.example.com' }]
});

// Mobile
const notification = app.pushNotification().mobile('appId');
await notification.sendAndroidNotification({ message: 'Test!', badge_count: 1 }, 'user@email.com');
await notification.sendIOSNotification({ message: 'Test!', badge_count: 1 }, 'user@email.com');
```

Node.js prerequisites: Enable push notifications in web app, user must allow/opt-in. Mobile requires registered app + Catalyst Authentication + platform-specific push config. [Source: catalyst-nodejs-sdk-cloud-scale-remaining.md]

**Key difference**: Java uses separate `ZCWebNotification` and `ZCMobileNotification` classes. Node.js unifies under `app.pushNotification()` with `.mobile('appId')` for mobile.

## Sources

- [[catalyst-java-sdk-cloud-scale-remaining]]
- [[catalyst-nodejs-sdk-cloud-scale-remaining]] — Node.js Push Notifications (3 pages)

## Related Concepts

- [[catalyst-mail]] — Email as alternative notification channel
- [[catalyst-authentication]] — Required for mobile push (user must be authenticated)

## Evolution

- Web notifications came first; mobile notifications added later with separate class hierarchy.

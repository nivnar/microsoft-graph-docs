---
title: "Get immutable identifiers for Outlook resources"
description: "Outlook items (messages, events, contacts, tasks) have an interesting behavior that you've probably either never noticed or has caused you significant frustration: their IDs change. It doesn't happen often, only if the item is moved, but it can cause real problems for apps that store IDs offline for later use. Immutable identifiers enables your application to obtain an ID that does not change for the lifetime of the item."
author: "angelgolfer-ms"
localization_priority: Priority
ms.prod: "outlook"
---

# Get immutable identifiers for Outlook resources

Outlook items (messages, events, contacts, tasks) have an interesting behavior that you've probably either never noticed or has caused you significant frustration: their IDs change. It doesn't happen often, only if the item is moved, but it can cause real problems for apps that store IDs offline for later use. Immutable identifiers enables your application to obtain an ID that does not change for the lifetime of the item.

> **Important:** Immutable identifiers are only available on the /beta version in Microsoft Graph.

## How it works

Immutable ID is an optional feature for Microsoft Graph. To opt in, your application needs to send an additional HTTP header in your API requests:

```http
Prefer: IdType="ImmutableId"
```

This header only applies to the request it is included with. If you want to always use immutable IDs, you must include this header with every API request.

## Lifetime of Immutable IDs

An item's immutable ID will not change so long as the item stays in the same mailbox. That means that immutable ID will NOT change if the item is moved to a different folder in the mailbox. However, the immutable ID will change if:

- The user moves the item to an archive mailbox
- The user exports the item (to a PST, as an MSG file, etc.) and re-imports it into their mailbox

## Items that support immutable ID

The following items support immutable IDs:

- [message resource type](/graph/api/resources/message?view=graph-rest-beta)
- [attachment resource type](/graph/api/resources/attachment?view=graph-rest-beta)
- [event resource type](/graph/api/resources/event?view=graph-rest-beta)
- [eventMessage resource type](/graph/api/resources/eventmessage?view=graph-rest-beta)
- [contact resource type](/graph/api/resources/contact?view=graph-rest-beta)
- [outlookTask resource type](/graph/api/resources/outlooktask?view=graph-rest-beta)

Container types (mailFolder, calendar, etc.) do not support immutable ID, but their regular IDs were already constant.

## Immutable ID with change notifications

You can request that Microsoft Graph send immutable IDs in change notifications by including the `Prefer: IdType="ImmutableId"` header when [creating a subscription](/graph/api/subscription-post-subscriptions?view=graph-rest-beta). Existing subscriptions created without the header will continue to use the default ID format. In order to switch existing subscriptions to use immutable IDs, you must delete and recreate them using the header.

## Immutable ID with delta query

You can request that Microsoft Graph return immutable IDs in [delta query responses](delta-query-overview.md) for supported resource types by including the `Prefer: IdType="ImmutableId"` header. The `nextLink` and `deltaLink` values returned by delta queries are compatible with both ID formats, so your application does not need to re-synchronize to take advantage of immutable ID. You can use the header to get immutable IDs going forward, and you can [update your app's storage](#updating-existing-data) separately.

## Updating existing data

If you've already got a database filled with thousands of regular IDs, you can migrate those IDs to immutable format using the [translateExchangeIds](/graph/api/user-translateexchangeids?view=graph-rest-beta) function. You can provide an array of up to 1000 IDs to be translated into a target format.

> **Note:** You can also use `translateExchangeIds` to migrate Exchange Web Services applications to Microsoft Graph.

### Example

The following example translates a normal Graph ID to an immutable Graph ID.

#### Request

```http
POST https://graph.microsoft.com/beta/me/translateExchangeIds

{
  "inputIds" :
  [
    "AQMkAGM2…"
  ],
  "targetIdType" : "restImmutableEntryId",
  "sourceIdType" : "restId"
}
```

#### Response

```http
HTTP 200 OK

{
  "value": [
    {
      "targetId": "AAkALgAA...",
      "sourceId": "AQMkAGM2..."
    }
  ]
}
```

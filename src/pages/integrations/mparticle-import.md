## Overview

Sending events from mParticle to Branch will allow you to attribute downstream conversions like purchases across web and app to Branch link clicks. Events imported from mParticle to Branch will be available wherever you can normally use events within Branch, including dashboard visualizations, Data Feeds, Universal Ads postbacks, Journeys targeting, Liveview and more.

This guide walks through the server-side integration for data import from mParticle to Branch. For the client-side integration, see the [mParticle iOS](/pages/apps/mparticle-ios/) or [android](/pages/apps/mparticle-android/) documentation. For data export from Branch to mParticle, go [here](/pages/integrations/mparticle). 

### What events does Branch import?

Branch will import events that are not auto-tracked with the Branch SDKs. This includes commerce, content, user lifecycle, and custom events, and excludes events like clicks and installs. See the full list of supported events and associated mappings [here](#supported-events). Branch will only import events that can be [tied to a user](#identifiers). 

## Setup

1. [Contact Branch](https://support.branch.io){:target="\_blank"} to configure Branch to receive events from mParticle. Please note that a subscription to [Data Feeds](https://branch.io/data-feeds/){:target="\_blank"} is required to enable data import from mParticle to Branch.
1. Navigate to the [Data Integrations page](https://dashboard.branch.io/data-import-export/data-feeds/integrations){:target="\_blank"} of the Branch dashboard.
    1. Select **mParticle** from the menu on the left.
    1. Select the platforms you would like to import events from and click **Save**. 
    
    ![image](/img/pages/integrations/mparticle/mparticle-import.png)

1. Navigate to your your mParticle UI’s Connections page.
    1. Select an input and add Branch as the output.
    1. Enter your Branch Key. This can be found in the [Account Settings > App](https://dashboard.branch.io/account-settings/app){:target="\_blank"} section of the Branch dashboard.
    1. Enter your Branch Secret. This can be found in the [Account Settings > App](https://dashboard.branch.io/account-settings/app){:target="\_blank"} section of the Branch dashboard.

For server-side event import, **you can ignore the SDK integration instructions**. 

!!! warning "Avoid duplicate data"
    To avoid duplicate data, you should either [track events directly with Branch](/pages/apps/v2event) or track events with mParticle and then enable import to Branch, not both. Branch will warn you if you try to import events to Branch that you are already tracking.


### Supported events

The Branch integration supports some of the events tracked with mParticle’s [API or SDKs](https://docs.mparticle.com/developers/server/json-reference/#events){:target="\_blank"}. Branch will only import events from mParticle that are not already auto-tracked with the Branch  SDK. This means that events like click and install cannot be imported. The following outlines how mParticle events are mapped to Branch events, and which will be imported to Branch:

| mParticle Event | Branch Event | Branch Event Category | Imported |
| --- | --- | --- | --- |
| CommerceEventType.ProductAddToCart | Add To Cart | Commerce Event | **Yes** |
| CommerceEventType.ProductAddToWishlist | Add To Wishlist | Commerce Event | **Yes** |
| *use the Branch event name* | View Cart | Commerce Event | **Yes** |
| *use the Branch event name* | Add Payment Info | Commerce Event | **Yes** |
| CommerceEventType.ProductCheckout | Initiate Purchase | Commerce Event | **Yes** |
| CommerceEventType.ProductPurchase | Purchase | Commerce Event | **Yes** |
| *use the Branch event name* | Spend Credits | Commerce Event | **Yes** |
| EventType.Search | Search | Content Event | **Yes** |
| CommerceEventType.ProductViewDetail | View Item | Content Event | **Yes** |
| *use the Branch event name* | View Items | Content Event | **Yes** |
| *use the Branch event name* | Rate | Content Event | **Yes** |
| *use the Branch event name* | Share | Content Event | **Yes** |
| *use the Branch event name* | Complete Registration | Lifecycle Event | **Yes** |
| *use the Branch event name* | Complete Tutorial | Lifecycle Event | **Yes** |
| *use the Branch event name* | Achieve Level | Lifecycle Event | **Yes** |
| *use the Branch event name* | Unlock Achievement | Lifecycle Event | **Yes** |
| *any event name* | Custom | Custom Event | **Yes** |
| - | Click | - | No |
| - | Install | - | No |
| - | Reinstall | - | No |
| - | Open | - | No |
| - | SMS Sent | - | No |
| - | Pageview | - | No |
| - | Web Session Start | - | No |
| - | Branch CTA View | - | No |
| - | Impression | - | No |
| - | Web to App Auto Redirect | - | No |


#### Identifiers

Identifiers are required for events to be imported to Branch. You must include:

* developer_identity OR
* os=iOS AND (idfa OR idfv) OR
* os=Android AND (android_id or aaid)

At this time, Branch [will not attribute logged out web events](#attribution-for-logged-out-users-on-web). 

#### Validating the integration

Once you have import turned on in both mParticle and Branch, events should come through. You will see a green dot on the import card if Branch has seen events:

![image](/img/pages/integrations/mparticle/mparticle-import-status.png)

To see more information on the events that are coming in, you can look at events with **origin** `MPARTICLE` in [Liveview](https://dashboard.branch.io/liveview/events){:target="\_blank"}:

![image](/img/pages/integrations/mparticle/mparticle-import-liveview.png)

Branch imports events from mParticle as [commerce, user lifecycle, content, or custom events](#supported-events).

#### Using imported events

Events imported from mParticle to Branch will be available wherever you can normally use events within Branch. This includes dashboard visualizations, Data Feeds (including Data Integrations, Query API, Webhooks, and Data Export API), Universal Ads postbacks, Journeys targeting, Liveview and more.

## Advanced

### Attribution for logged out users on web

Branch uses a custom, in-house identifier for logged out users on web. If you enable the server to server integration from mParticle to Branch, you will not be able to attribute logged out web events to a campaign run with Branch. For this reason, you may want to track web events directly with the Branch web SDK, while still sending app events server to server from mParticle. Branch allows you to then toggle web event import off to prevent duplicate data:

---
nav_title: Safari Mobile Web Push
article_title: Safari Mobile Web Push
platform: Web
channel: push
page_order: 5
page_type: reference
description: "This reference article cover how to integrate web push on your iOS and iPad Safari browsers."
search_rank: 3
---

# Safari Mobile Web Push (iOS and iPadOS)

> [Safari v16.4][safari-release-notes] supports mobile web push, which means you can now re-engage mobile users with push notifications on iOS and iPadOS.<br><br>This article will guide you through the steps required to set up mobile push for safari.

## Integration steps

First, please read and follow our standard [web push integration guide][web-push-integration]. The following steps are only required to support web push on Safari for iOS and iPadOS support.

### Step 1: Create a manifest file {#manifest}

A [Web Application Manifest][manifest-file] is a JSON file that controls how your website is presented when installed to a user's home screen.

For example, you can set the background theme color and icon that the [App Switcher][app-switcher] uses, whether it renders as full screen to resemble a native app, or whether the app should open in landscape or portrait mode.

Create a new `manifest.json` file in your website's root directory, with the following mandatory fields. 

```json
{
  "name": "your app name",
  "short_name": "your app name",
  "display": "fullscreen",
  "icons": [{
    "src": "favicon.ico",
    "sizes": "128x128",
  }]
}
```

The full list of supported fields can be found [here][manifest-file].

### Step 2: Link the manifest file {#manifest-link}

Add the following `<link>` tag to your website's `<head>` element pointing to where your manifest file is hosted.

```html
<link rel="manifest" href="/manifest.json" />
```

### Step 3: Add a service worker {#service-worker}

Your website must have a service worker file that imports the Braze service-worker library, as described in our [web push integration guide][service-worker].

### Step 4: Add to home screen {#add-to-homescreen}

Unlike major browsers like Chrome and Firefox, you are not allowed to request push permission on Safari iOS/iPadOS unless your website has been added to the user's home screen. 

The [Add to Homescreen][add-to-homescreen] feature lets users bookmark your website, adding your icon to their valuable home screen real estate.

![An iphone showing options to bookmark a website and save to the home screen][add-to-homescreen-img]{: style="max-width:40%"}

### Step 5: Show the native push prompt {#push-prompt}
Once the app has been added to your home screen you can now request push permission when the user takes an action (such as clicking a button). This can be done using the [`requestPushPermission`][requestPushPermission] method, or with a [no-code push primer in-app message][push-primer].

{% alert note %}
Once you accept or decline the prompt, you'll need to delete and reinstall the website to your home screen to be able to show the prompt again.
{% endalert %}

![A push prompt asking to "allow" or "don't allow" Notifications][2]{: style="max-width:40%"}

For example:

```typescript
import { requestPushPermission } from "@braze/web-sdk";

button.onclick = function(){
    requestPushPermission(() => {
        console.log(`User accepted push prompt`);
    }, (temporary) => {
        console.log(`User ${temporary ? "temporarily dismissed" : "permanently denied"} push prompt`);
    });
};
```


## Next steps

Next, send yourself a [test message][test-message] to validate the integration. Once your integration is complete you can use our [no-code push primer messages][push-primer] to optimize your push opt-in rates.

[webkit-release-notes]: https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/
[safari-release-notes]: https://developer.apple.com/documentation/safari-release-notes/safari-16_4-release-notes
[manifest-file]: https://developer.mozilla.org/en-US/docs/Web/Manifest
[app-switcher]: https://support.apple.com/en-us/HT202070
[add-to-homescreen]: https://support.apple.com/guide/iphone/bookmark-favorite-webpages-iph42ab2f3a7/ios#iph4f9a47bbc
[web-push-integration]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/push_notifications/integration/
[service-worker]: {{site.baseurl}}/developer_guide/platform_integration_guides/web/push_notifications/integration/#step-1-configure-your-sites-service-worker
[test-message]: {{site.baseurl}}/user_guide/engagement_tools/campaigns/testing_and_more/sending_test_messages/
[push-primer]: {{site.baseurl}}/user_guide/message_building_by_channel/push/push_primer_messages/
[requestPushPermission]: https://js.appboycdn.com/web-sdk/latest/doc/modules/braze.html#requestpushpermission
[1]: {% image_buster /assets/img/push_implementation_guide/add-to-homescreen.png %}
[2]: {% image_buster /assets/img/push_implementation_guide/safari-mobile-push-prompt.png %}

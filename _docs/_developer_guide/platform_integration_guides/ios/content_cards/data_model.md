---
nav_title: Data Model
platform: iOS
page_order: 5
search_rank: 5
---

# Content Cards Data Model
The Content Cards data model is available in the iOS SDK.

## Getting the Data

To access the Content Cards data model, subscribe to Content Cards update events:

{% tabs %}
{% tab OBJECTIVE-C %}
```objc
// Subscribe to content cards updates
// Note: you should remove the observer where appropriate
[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(contentCardsUpdated:)
                                             name:ABKContentCardsProcessedNotification
                                           object:nil];
```

```objc
// Called when content cards are refreshed (via `requestContentCardsRefresh`)
- (void)contentCardsUpdated:(NSNotification *)notification {
  BOOL updateIsSuccessful = [notification.userInfo[ABKContentCardsProcessedIsSuccessfulKey] boolValue];
  if (updateIsSuccessful) {
    // get the cards using [[Appboy sharedInstance].contentCardsController getContentCards];
  }
}
```
{% endtab %}
{% tab swift %}
```swift
// Subscribe to content card updates
// Note: you should remove the observer where appropriate
NotificationCenter.default.addObserver(self, selector:
  #selector(contentCardsUpdated),
  name:NSNotification.Name.ABKContentCardsProcessed, object: nil)
```

```swift
// Called when the content cards are refreshed (via `requestContentCardsRefresh`)
@objc private func contentCardsUpdated(_ notification: Notification) {
  if let updateIsSuccessful = notification.userInfo?[ABKContentCardsProcessedIsSuccessfulKey] as? Bool {
    if (updateIsSuccessful) {
      // get the cards using Appboy.sharedInstance()?.contentCardsController.contentCards
    }
  }
}
```
{% endtab %}
{% endtabs %}

If you want to change the card data after it's been sent by Braze, we recommend storing a deep copy of the card data locally, updating the data and displaying yourself. The cards are accessible via [ABKContentCardsController](https://appboy.github.io/appboy-ios-sdk/docs/interface_a_b_k_content_cards_controller.html).

## Content Card Model

Braze offers three content card types: Banner, Captioned Image and Classic. Each type inherits common properties from a base ABKContentCard class, plus has additional properties as described below.

### Base Content Card Model Properties - ABKContentCard.

|Model|Description|
|---|---|
|`idString` | (read only) The card's ID set by Braze. |
| `viewed` | This property reflects if the card is viewed or not viewed by the user|
| `created` | (read only) This property is the unix timestamp of the card's creation time from Braze. |
| `expiresAt` | (read only) This property is the unix timestamp of the card's expiration time.|
| `dismissible` | This property reflects if the card can be dismissed by the user.|
| `pinned` | This property reflects if the card has been pinned by the user.|
| `dismissed` | This property reflects if the card has been dismissed by the user.|
| `url` | The URL that will be opened after the card is clicked on. It can be a http(s) URL or a protocol URL.|
| `openURLInWebView` | This property determines whether the URL will be opened within the app or in an external web browser.|
| `extras`| An optional NSDictionary of NSString values.|

### Banner Content Card Properties - ABKBannerContentCard

|Model|Description|
|---|---|
| `image` | This property is the URL of the card's image.|
| `imageAspectRatio` | This property is the aspect ratio of the card's image.|

### Captioned Image Content Card Properties - ABKCaptionedImageCard

|Model|Description|
|---|---|
| `image` | This property is the URL of the card's image.|
| `imageAspectRatio` | This property is the aspect ratio of the card's image.|
| `title` | The title text for the card.|
| `cardDescription` | The body text for the card.|
| `domain` | The link text for the property URL, like @"blog.braze.com". It can be displayed on the card's UI to indicate the action/direction of clicking on the card.|

### Classic Content Card Properties - ABKClassicContentCard

|Model|Description|
|---|---|
| `image` | (optional) This property is the URL of the card's image.|
| `title` | The title text for the card. |
| `cardDescription` | The body text for the card. |
| `domain` | The link text for the property url, like @"blog.braze.com". It can be displayed on the card's UI to indicate the action/direction of clicking on the card. |

## Card Methods

|Method|Description|
|---|---|
| `logContentCardImpression` | Manually log an impression to Braze for a particular card. |
| `logContentCardClicked` | Manually log a click to Braze for a particular card. The SDK will only log a card click when the card has the `url` property with a valid value. |
| `logContentCardDismissed` | Manually log a dismissal to Braze for a particular card.|

## Log Content Cards Display

When displaying the Content Cards in your own user interface, you can manually record Content Cards impressions via the method `logContentCardsDisplayed;` on the `Appboy` interface. For example:

```objc
[[Appboy sharedInstance] logContentCardsDisplayed];
```

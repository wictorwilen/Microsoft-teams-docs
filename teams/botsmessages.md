# Reference: Messages, cards, and actions in Microsoft Teams

A conversation is a series of messages sent between your bot and one or more users. Each message is an `Activity` object. When a user sends a message, the channel on which they're communicating posts the message to your bot (web service). Your bot examines the message to determine its type and responds accordingly.

Most content sent between a user and your bot uses `messageType: message`. For event-style messages, please review [Microsoft Teams bot events](botevents.md). Note that Speech is currently not supported.

For more information about core messaging functionality of the Bot Framework, please review [Create messages](https://docs.microsoft.com/en-us/bot-framework/rest-api/bot-framework-rest-connector-create-messages) and [Send and receive messages](https://docs.microsoft.com/en-us/bot-framework/rest-api/bot-framework-rest-connector-send-and-receive-messages) in the Bot Framework documentation and see [Bot Builder samples](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/cards-RichCards) on GitHub.

## Messages

Most messages to and from your bot are of type `message`. Your bot can send rich text, pictures and cards. Users can send rich text and pictures to your bot. You can specify the type of content your bot can handle in the Microsoft Teams settings page for your bot.

| Format | From user to bot  | From bot to user |  Notes |
| --- | --- | --- | --- |
| Rich text | ✔ | ✔ |  |
| Pictures | ✔ | ✔ | Maximum 1024×1024 and 1 MB in PNG, JPEG, or GIF format; animated GIF not officially supported |
| Cards | ✘ | ✔ | Teams currently supports hero, thumbnail, and Office 365 Connector cards |
| Emojis | ✘ | ✔ | Teams currently supports emojis via UTF-16 (such as U+1F600 for grinning face) |

### Message format

You can set the optional [`TextFormat`](https://docs.microsoft.com/en-us/bot-framework/dotnet/bot-builder-dotnet-create-messages#message-text-and-format) property to control how your message's text content is rendered.

Microsoft Teams supports the following formatting options:

| TextFormat value | Description |
| --- | --- |
| plain | The text should be treated as raw text with no formatting applied at all |
| markdown | The text should be treated as Markdown formatting and rendered on the channel as appropriate; see [Message text and format](https://docs.microsoft.com/en-us/bot-framework/dotnet/bot-builder-dotnet-create-messages#message-text-and-format) for supported styles |
| xml | The text is simple XML markup; see [Message text and format](https://docs.microsoft.com/en-us/bot-framework/dotnet/bot-builder-dotnet-create-messages#message-text-and-format) for supported styles |

>**Note:** On hero and thumbnail cards, message format is supported only on the text property. Formatting is not supported on the title and subtitle properties at this time.

### Picture messages

Pictures are sent by adding attachments to a message.  You can find more information on attachments in the [Bot Framework documentation](https://docs.botframework.com/en-us/core-concepts/attachments/).

Pictures can be at most 1024×1024 and 1 MB in PNG, JPEG, or GIF format; animated GIF is not officially supported.

We recommend that you specify the height and width of each image by using XML. If you use Markdown, the image size defaults to 256×256. For example:

* Use `<img src="http://aka.ms/Fo983c" alt="Duck on a rock" height="150" width="223"></img>`
* Don't use `![Duck on a rock](http://aka.ms/Fo983c)`

## Cards 

Microsoft Teams supports rich cards, which can have several properties and attachments.

Supported:
* Hero card
* Thumbnail card

Supported with modifications:
* Office 365 Connector card&mdash;the `HttpPost` and `ActionCard` actions and the `heroImage` field are not currently supported, and Office 365 Connectors do not render properly in iOS.
* Sign-in card&mdash;the `signin` action is not supported. You can replace the button action with `openUrl` to get the desired result.

Not supported:
* Receipt card
* Media cards (Animation, Audio, Video)

Additionally, we support the following layouts:
* Horizontal carousel layout
* Vertical list layout

Both layouts support hero and thumbnail cards.

You can find information on how to use cards in the documentation for the Bot Builder SDK. Code samples are in the Microsoft/BotBuilder-Samples repository on GitHub:
* .NET
  * [Add rich card attachments to messages](https://docs.microsoft.com/en-us/bot-framework/dotnet/bot-builder-dotnet-add-rich-card-attachments)
  * [Rich cards sample code](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/cards-RichCards)
* Node.js
  * [Add rich card attachments to messages](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-send-rich-cards)
  * [Rich cards sample code](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/cards-RichCards)

### Inline card images

Your card can contain inline images by including a link to the image content hosted on a public CDN.

Images are scaled up or down in size while maintaining the aspect ratio to cover the image area, and then cropped from center to achieve the image aspect ratio for the card.

Images must be at most 1024×1024 and 1 MB in PNG, JPEG, or GIF format; animated GIF is not officially supported.

| Property | Type  | Description |                                                           
| --- | --- | --- |
| url | URL | HTTPS URL to the image |
| alt | String | Accessible description of the image |

### Hero card

The [hero card](https://docs.botframework.com/en-us/csharp/builder/sdkreference/attachments.html#herocard) renders a title, subtitle, text, large image, and buttons.

![Example of a hero card](images/Cards/hero.png)

| Property | Type  | Description |                                                           
| --- | --- | --- |
| title | Rich text | Title of the card. Maximum 2 lines; formatting not currently supported |
| subtitle | Rich text | Subtitle of the card. Maximum 2 lines; formatting not currently supported |
| text | Rich text | Text appears just below the subtitle; see [Message format](#message-format) for formatting options |
| images | Array of images | Image displayed at top of card. Aspect ratio 16:9 |
| buttons | Array of action objects | Set of actions applicable to the current card. Maximum 6 |
| tap | Action object | This action will be activated when the user taps on the card itself |

### Thumbnail card

The [thumbnail card](https://docs.botframework.com/en-us/csharp/builder/sdkreference/attachments.html#thumbnailcard) renders a title, subtitle, text, small thumbmail image, and buttons.

![Example of a thumbnail card](images/Cards/thumbnail.png)

| Property | Type  | Description |                                                           
| --- | --- | --- |
| title | Rich text | Title of the card. Maximum 2 lines; formatting not currently supported |
| subtitle | Rich text | Subtitle of the card. Maximum 2 lines; formatting not currently supported |
| text | Rich text | Text appears just below the subtitle; see [Message format](#message-format) for formatting options |
| images | Array of images | Image displayed at top of card. Aspect ratio 1:1 (square) |
| buttons | Array of action objects | Set of actions applicable to the current card. Maximum 6 |
| tap | Action object | This action will be activated when the user taps on the card itself |

### Office 365 Connector card

The Office 365 Connector card provides a more flexible layout with multiple sections, images, and fields.

>**Note:** An Office 365 Connector card can display a maximum of 10 sections. Any additional sections do not appear.

![Example of an Office 365 Connector card](images/Cards/o365connector.png)

See the [Actionable message card reference](https://docs.microsoft.com/en-us/outlook/actionable-messages/card-reference) for details about card fields and actions. Teams currently does not support the following:
* Fields: `heroImage`
* Actions: `HttpPOST` and `ActionCard`

You can use the O365ConnectorCard class in the [Microsoft Teams extensions for the Bot Builder SDK](code.md#microsoft-teams-extensions-for-the-bot-builder-sdk) to send this card from your bot.

### Carousel layout

The [carousel layout](https://docs.microsoft.com/en-us/bot-framework/dotnet/bot-builder-dotnet-add-rich-card-attachments) can be used to show a carousel of cards, with associated action buttons.

>**Note:** A carousel can display a maximum of 10 cards per message.

![Example of a carousel of cards](images/Cards/carousel.png)

Properties are the same as for the hero or thumbnail card.

### List layout

The list layout can be used to show a vertically stacked list of cards.

>**Note:** A list can display a maximum of 10 cards per message.

![Example of a list of cards](images/Cards/list.png)

Properties are the same as for the hero or thumbnail card.

>**Note:** Some combinations of list cards are not yet supported on iOS and Android.

### Buttons

Buttons are shown stacked at the bottom of the card. Button text is always on a single line and will be truncated if the text exceeds the button width. Any additional buttons beyond the maximum number supported by the card will not be shown.

## Card actions

Teams supports the following activity ([`CardAction`](https://docs.microsoft.com/en-us/bot-framework/dotnet/bot-builder-dotnet-add-rich-card-attachments#process-events-within-rich-cards)) types.

| Type | Action | 
| --- | --- |
| `openUrl` | Opens a URL in the built-in browser. |
| `messageBack` | Sends a message and payload to the bot (from the user who clicked the button or tapped the card) and sends a separate message to the chat stream. |
| `imBack`| Sends a message to the bot (from the user who clicked the button or tapped the card). This message (from user to bot) is visible to all conversation participants. |
| `invoke` | Sends a message and payload to the bot (from the user who clicked the button or tapped the card). This message is not visible. |

>**Notes**
>* Teams does not support `CardAction` types not listed in the preceding table.
>* Teams does not support the `SuggestedActions` property.

### openUrl

This action type specifies a URL to launch in the default browser.  Note that your bot does not receive any notice on which button was clicked.

The `value` field must contain a full and properly formed URL.

```json
{
    "type": "openUrl",
    "title": "Tabs in Teams",
    "value": "https://msdn.microsoft.com/en-us/microsoft-teams/tabs"
}
```

### messageBack

>**New**

With `messageBack`, you can create a fully customized action with the following properties:

| Property | Description |
| --- | --- |
| `title` | Appears as the button label. |
| `displayText` | Optional. Echoed by the user into the chat stream when the action is performed. This text is *not* sent to your bot. |
| `value` | Sent to your bot when the action is performed. You can encode context for the action, such as unique identifiers or a JSON object. |
| `text` | Sent to your bot when the action is performed. Use this property to simplify bot development: Your code can check a single top-level property to dispatch bot logic. |

The flexibility of `messageBack` means that your code can choose not to leave a visible user message in the history simply by not using `displayText`.

```json
{
  ...
  "buttons": [
    {
    "type": "messageBack",
    "title": "My MessageBack button",
    "displayText": "I clicked this button",
    "text": "User just clicked the MessageBack button",
    "value": "{\"property\": \"propertyValue\" }"
    }
  ]
}
```

The `value` property can be either a serialized JSON string or a JSON object.

#### Inbound message schema example
```json
{
   "text":"User just clicked the MessageBack button",
   "value":{
      "property":"propertyValue"
   },
   "type":"message",
   "timestamp":"2017-06-22T22:38:47.407Z",
   "id":"f:5261769396935243054",
   "channelId":"msteams",
   "serviceUrl":"https://smba.trafficmanager.net/amer-client-ss.msg/",
   "from":{
      "id":"29:102jd210jd010icsoaeclaejcoa9ue09u",
      "name":"John Smith"
   },
   "conversation":{
      "id":"19:malejcou081i20ojmlcau0@thread.skype;messageid=1498171086622"
   },
   "recipient":{
      "id":"28:76096e45-119f-4736-859c-6dfff54395f7",
      "name":"MyBot"
   },
   "entities":[
      {
         "locale":"en-US",
         "country":"US",
         "platform":"Windows",
         "type":"clientInfo"
      }
   ],
   "channelData":{
      "channel":{
         "id":"19:malejcou081i20ojmlcau0@thread.skype"
      },
      "team":{
         "id":"19:12d021jdoijsaeoaue0u@thread.skype"
      },
      "tenant":{
         "id":"bec8e231-67ad-484e-87f4-3e5438390a77"
      }
   }
}
```

### imBack

This action triggers a return message to your bot, as if the user typed it in a normal chat message.  Thus, your user, and all other users if in a channel, will see that button response.

The `value` field should contain the text string echoed in the chat and therefore sent back to the bot.  This is the message text you will process in your bot to perform the desired logic.  Note: this field is a simple string - there is no support for formatting or hidden characters.

```json
{
    "type": "imBack",
    "title": "More",
    "value": "Show me more"
}
```

### invoke

>**Note:** We recommend that you use `messageBack` instead of `invoke`.

The `invoke` message type silently sends a JSON payload that you define to your bot.  This is useful if you want to send more detailed information back to your bot without having to send via a simple `imBack` text string.  Note that the user, in 1:1 or in channel, sees no notification as a result of their click.

The `value` field will contain a stringified JSON object.  You can include identifiers or any other context necessary to carry out the operation.

```json
{
    "type": "invoke",
    "title": "Option 1",
    "value": { 
        "option": "opt1" 
    } 
}
```

When a user clicks the button, your bot will receive the predefined `value` object with some additional info. Please note that the activity type will be `invoke` instead of `message` (activity.Type == "invoke").

#### Example: Invoke button definition (.NET)

```csharp
var button = new CardAction()
{
    Title = "Option 1",
    Type = "invoke",
    Value = "{\"option\": \"opt1\"}"
};
```

#### Example: Invoke inbound schema

```json
{
    "type": "invoke",
    "value": {
        "option": "opt1"
    },
    "timestamp": "2017-02-10T04:11:19.614Z",
    "localTimestamp": "2017-02-09T21:11:19.614-07:00",
    "id": "f:6894910862892785420",
    "channelId": "msteams",
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",
    "from": {
        "id": "29:1Eniglq0-uVL83xNB9GU6w_G5a4SZF0gcJLprZzhtEbel21G_5h-
    NgoprRw45mP0AXUIZVeqrsIHSYV4ntgfVJQ",
        "name": "Richard Moe"
    },
    "conversation": {
        "id": "19:97b1ec61-45bf-453c-9059-6e8984e0cef4_8d88f59b-ae61-4300-bec0-caace7d28446@unq.gbl.spaces"
    },
    "recipient": {
        "id": "28:8d88f59b-ae61-4300-bec0-caace7d28446",
        "name": "MyBot"
    },
    "entities": [
        {
            "locale": "en-US",
            "country": "US",
            "platform": "Web",
            "type": "clientInfo"
        }
    ],
    "channelData": {
        "channel": { 
            "id": "19:dc5ba12695be4eb7bf457cad6b4709eb@thread.skype" 
        },
        "team": { 
            "id": "19:712c61d0ef384e5fa681ba90ca943398@thread.skype" 
        },
        "tenant": { 
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47" 
        }
    }
}
```

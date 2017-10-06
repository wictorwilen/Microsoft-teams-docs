# Authenticate a user in your Microsoft Teams app

Intro TK

## Authentication in bots

TK

## Authentication in tabs

The tab [configuration](createconfigpage.md) and [content](createcontentpage.md) pages run in iframes. Azure Active Directory (Azure AD), and other identity providers that you may use, do not usually allow their sign-in and consent pages to be hosted within an iframe.  Because of this, your app needs to authenticate the user in a pop-up window. You must use the [Microsoft Teams JavaScript library](jslibrary.md) to do this, so that it works successfully in both the web and desktop apps for Microsoft Teams.  

1. Add UI to your configuration or content page to enable the user to sign in when necessary. You should not drive the authentication pop-up without user action, because this is likely to trigger the browser's pop-up blocker.

2. Add the domain of your authentication URL to the [`validDomains`](schema.md#validdomains) section of the manifest. Failure to do so might result in an empty pop-up.

3. You can [get user context information](getusercontext.md) to help build authentication requests and URLs. For example, you can use the user's name (upn) as the `login_hint` value for Azure AD sign-in, which means the user might need to type less, or even that sign-in completes with no user action at all. Note that you should *not* use this context directly as proof of identity. For example, an attacker could load your page in a "malicious browser" and provide it with any information they want.

4. When the user selects to sign in, call `microsoftTeams.authentication.authenticate({url: <auth URL>, width: <width>, height: <height>, successCallback: <successCallback>, failureCallback: <failureCallback>})`.
	
   This launches the pop-up window in which the authentication should occur. This page must be hosted on your domain and listed in the [`validDomains`](schema.md#validdomains) section of the manifest. Within this page, you should redirect to your identity provider, so the user can sign in. After the user completes authentication, the pop-up window is redirected to the callback page you specified for your app. Ensure that the authentication
   
5. In your app's callback page, call `microsoftTeams.authentication.notifySuccess()` or `microsoftTeams.authentication.notifyFailure()`.
	
   This results in a callback to the `successCallback` or `failureCallback` function that you registered in step 4, inside the original configuration or content page.

6. If successful, you can refresh or reload the page and show the configuration or content relevant to the now-authenticated user. If authentication fails, display an error message.

7. Your app can set its own session cookie in the usual way so that the user need not sign in again when they return to your tab on the current device.

>**Note:** Because `navigateCrossDomain` isn't supported in the authentication window, we recommend that your authentication start and end domains be the same as your content domain. In the future, we will allow the authentication start and end domains be separate from the content, but for forward compatibility, we recommand you put all on the same domain.

## Authentication in compose extensions

If your service requires user authentication, you need to sign in the user before he or she can use the compose extension. If you have written a bot or a tab that signs in the user, this section should be familiar.

The sequence is as follows:

1.	User issues a query, or the default query is automatically sent to your service.
2.	Your service checks whether the user has first authenticated by inspecting the Teams user ID.
3.	If the user has not authenticated, send back a `login` action including the authentication URL.
4.	The Microsoft Teams client launches a pop-up window hosting your webpage using the given authentication URL.
5.	After the user signs in, you should close your window and send an "authentication code" to the Teams client.
6.	The Teams client then reissues the query to your service, which includes the authentication code passed in step 5.

Your service should verify that the authentication code received in step 6 matches the one from step 5.  This ensures that a malicious user does not try to spoof or compromise the sign-in flow.  This effectively "closes the loop" to finish the secure authentication sequence.

### Respond with a sign-in action

To prompt an unauthenticated user to sign in, respond with a suggested action that includes the authentication URL.

#### Response example

```json
{
  "composeExtension":{
    "type":"auth",
    "channelData":{
    },
    "suggestedActions":[
      {
        "type": "openApp",
        "value": "https://example.com/auth",
        "title": "Sign in to this app"
      }
    ]
  }
}
```

>**Note:** For the sign-in experience to be hosted in a Teams pop-up, the domain portion of the URL must be in your app’s list of valid domains. (See [validDomains](schema.md#validdomains) in the manifest schema.)

### Start the sign-in flow

Your sign-in experience should be responsive and fit within a popup window. It should integrate with the [Microsoft Teams JavaScript library](jslibrary.md), which uses message passing.

As with other embedded experiences running inside Microsoft Teams, your code inside the window needs to first call `microsoftTeams.initialize()`. If your code performs an OAuth flow, you can pass the Teams user ID into your window, which then can pass it to the OAuth sign-in URL.

### Complete the sign-in flow

When the sign-in request completes and redirects back to your page, it should perform the following steps:

1.	Generate a security code. (This can be a random number.) You need to cache this code on your service, along with the credentials obtained through the sign-in flow (such as OAuth 2.0 tokens).
2.	Call `microsoftTeams.authentication.notifySuccess` and pass the security code.

At this point, the window closes and control is passed to the Teams client. The client now can reissue the original user query, along with the security code in the `state` property. Your code can use the security code to look up the credentials stored earlier to complete the authentication sequence and then complete the user request.

#### Reissued request example

```json
{
    "name": "composeExtension/query",
    "value": {
        "commandId": "insertWiki",
        "parameters": [{
            "name": "searchKeyword",
            "value": "lakers"
        }],
        "state": "12345",
        "queryOptions": {
            "skip": 0,
            "count": 25
        }
    },
    "type": "invoke",
    "timestamp": "2017-04-26T05:18:25.629Z",
    "localTimestamp": "2017-04-25T22:18:25.629-07:00",
    "entities": [{
        "locale": "en-US",
        "country": "US",
        "platform": "Web",
        "type": "clientInfo"
    }],
    "text": "",
    "attachments": [],
    "address": {
        "id": "f:7638210432489287768",
        "channelId": "msteams",
        "user": {
            "id": "29:1A5TJWHkbOwSyu_L9Ktk9QFI1d_kBOEPeNEeO1INscpKHzHTvWfiau5AX_6y3SuiOby-r73dzHJ17HipUWqGPgw"
        },
        "conversation": {
            "id": "19:7705841b240044b297123ad7f9c99217@thread.skype"
        },
        "bot": {
            "id": "28:c073afa8-7e77-4f92-b3e7-aa589e952a3e",
            "name": "maotestbot2"
        },
        "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",
        "useAuth": true
    },
    "source": "msteams"
}
```

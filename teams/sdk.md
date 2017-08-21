# Developing your app using the Teams SDKs

These libraries are built and maintained by Teams to aid in the development of apps on our platform.

## .NET and Node.JS SDKs

The Teams .NET and Node.JS SDKs make it easy to develop your app using our APIs. These libraries extend the basic BotBuilder  classes and methods with:
* Specialized Teams card types like O365 Connector Card
* Consuming and setting Teams-specific channel data on Activities
* Processing Compose Extension requests
* Handling rate limiting

### Setting up for development
You can find the full source code on [Github](https://github.com/OfficeDev/BotBuilder-MicrosoftTeams).
* For .NET, install the [Microsoft.Bot.Connector.Teams](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams) package to your Visual Studio project.
* For Node.js, add the [botbuilder-teams](https://www.npmjs.com/package/botbuilder-teams) NPM package.

Both will install package dependencies, namely BotBuilder.

## JavaScript Library

Use the JavaScript library to integrate your web application with Teams. This library provides methods for your tab, as well as your authentication and configuration experiences.

The full documentation for the JS library is available [here](jslibrary.md).
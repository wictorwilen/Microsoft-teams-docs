# Create the package for your Microsoft Teams app

App experiences in Teams are defined by their app manifest, and bundled in an app package for use in sideloading or Office Store submission.  You'll need an app package to test your experience in Teams, via the sideloading process documented [here](sideload.md).

A Teams app package is a .zip file containing:
* A manifest file named "manifest.json", which specifies attributes of your app and points to required resources for your experience, such the location of its tab configuration page or the Microsoft app ID for its bot.
* A transparent "outline" icon and a full "color" icon.  See [Icons](#icons) later in this topic for more information.

>**Tip:** Download our [Simple Bot Package](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/SimpleBotPackage.zip) or [Full App Package](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/FullAppPackage.zip) to get started. Each package contains a template manifest with fake data and sample icons suitable for sideloading. These sample packages will not load as-is; you must customize them.

>**Tip 2**: If you would rather not handcraft the manifest.json file, and you're programming in TypeScript/Node.JS, consider using the [Microsoft Teams app Yeoman generator](https://www.npmjs.com/package/generator-teams). It will guide you through a wizard and create the manifest and code for a fully functional "skeleton app" to get started. The generated project solution includes Gulp tasks that automatically builds the manifest and project.

## Creating a manifest

Your manifest file must be named "manifest.json" and be at the top level of the sideload package.  Note that manifests and packages built previously might support an older version of the schema.  For Teams apps and especially Store submission, you must use the current [manifest schema](schema.md). 

> **Tip:** Specify the schema at the beginning of your manifest to enable IntelliSense or similar support from your code editor:
> 
> `"$schema": "https://statics.teams.microsoft.com/sdk/v1.0/manifest/MicrosoftTeams.schema.json",`

## Icons

Microsoft Teams requires two icons for your app experience, to be used within the product. Icons must be included in the package and referenced via relative paths in the manifest. The maximum length of each path is 2048 bytes.

### color

The `color` icon is used throughout Microsoft Teams (in app and tab galleries, bots, flyouts, and so on).  This icon should be 96x96 pixels.  If it has transparency, the `accentColor` will be used as the background.

### outline

The `outline` icon will be used in three specific places:  the app bar, pinnned compose extensions, and chiclets.  This icon must be 20x20; it needs to be a trace image using white, and use a transparent background.  Here are a few good examples:

!["Sample outline icons"](images/icons/sample20x20s.png)

For example, say your company is Contoso.  You'd submit two icons:

!["Sample Contoso icons"](images/icons/contosoicons.png)

Here's how the icons would appear in the UI:
#### Bot and chiclet in Channel view

!["Bot and chiclet UX"](images/icons/botandchiclet.png)

#### Flyout

!["Sample Contoso icons"](images/icons/flyout.png)

#### App bar and home screen

!["Sample Contoso icons"](images/icons/appbarhomescreen.png)
 
> Hitting problems?  See the [troubleshooting guide](troubleshooting.md).

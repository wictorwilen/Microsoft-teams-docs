# Create the content for your Microsoft Teams tab

The content page is an HTML page that you host.  When the user visits your tab, Microsoft Teams will load the `contentUrl` (that you [provided earlier](createconfigpage.md)) within an iframe inside the main tab canvas area.

In this page, you present the main function of your tab, following the [design recommendations](design.md#building-a-great-tab).  You may also need to use the [supplied context](getusercontext.md) to help display the correct content.

>**Note:** The very simple 'maps' example in this documentation uses existing Bing and Google maps as content pages for illustration, which of course do not include this library.  See the [samples](samples.md) for a full example tab that does so.  

![Tab with iframed content highlighted.](images/tab_content.png)

<!-- TODO: fix to use latest sample app, and remove note when done --> 

## Prerequisites for content displayed in your tab

For your content to display within a Microsoft Teams tab, make sure it meets the [requirements for tab pages](prerequisites.md).

>In summary: you must host your page on a secure https:// endpoint, ensure your page permits itself to be iframed, include the [Microsoft Teams Tab library](jslibrary.md), and call `microsoftTeams.initialize()`;

## Deep links to items within your content page

You can enable team members to create and share links to items within your tab - such as an individual task within a tab that contains a task list.  When clicked, the link will navigate to your tab, which focuses on the specific item.  [Find out how](deeplinks.md).

## Samples

Check out our [sample tabs](samples.md) on GitHub.

## Media format support for audio and video content

Microsoft Teams desktop client only supports the following media formats for audio and video content.

* Vorbis - http://www.vorbis.com/ 
* PCM_U8, PCM_S16LE, PCM_S32LE, PCM_f32LE, PCM_S16BE, PCM_S24BE, PCM_MULAW - https://wiki.multimedia.cx/?title=PCM 
* OGG - http://www.vorbis.com/ 
* Matroska - https://www.matroska.org/ 
* Wav - https://en.wikipedia.org/wiki/WAV 
* AAC - https://en.wikipedia.org/wiki/Advanced_Audio_Coding 
* H264 - https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC 
* MP3 - https://en.wikipedia.org/wiki/MP3 
* OPUS - http://opus-codec.org/ 
* MP4 - https://en.wikipedia.org/wiki/MPEG-4 


# Requirements for Microsoft Teams tabs app pages

> Please note: use of the Microsoft Teams tab SDK is subject to the [Terms of Use](https://aka.ms/bf-terms), [Privacy Statement](https://aka.ms/bf-privacy), and [Code of Conduct](https://aka.ms/bf-conduct) for the Microsoft Bot Framework (Preview).

All tab content, including configuration, content and tab removal pages must meet the following requirements:

* Pages must be hosted on a secure https:// endpoint.  Microsoft Teams will not display insecure http:// content.

* Your content must work in an iframe. By default web pages can be iframed by anyone. You may optionally set these headers if you wish to only allow your page to be iframed by Microsoft Teams for extra security:
	
	* Set header `Content-Security-Policy: frame-ancestors teams.microsoft.com *.teams.microsoft.com *.skype.com`. Most modern browsers support this.

		* For Internet Explorer 11 compatability, Set X-Content-Security-Policy as well.

	* Alternately, set header `X-Frame-Options: ALLOW-FROM https://teams.microsoft.com/`. This header is deprecated but still respected by most browsers.

* Include the [Microsoft Teams Tab library](jslibrary.md) in your page as a script source.

	`<script src="https://statics.teams.microsoft.com/sdk/v1.0/js/MicrosoftTeams.min.js" />`

* Once your page has successfully loaded, call `microsoftTeams.initialize()` to display your page. Microsoft Teams will not display your page unless you do so.

* All domains for pages you display in your tab(s) must be listed in the manifest's `validDomains` list.  See [here](schema.md#validdomains) for more information.

> Hitting problems?  See the [troubleshooting guide](troubleshooting.md).

>**Tip:** For developers using TypeScript, Microsoft Teams provides a [definition file](https://statics.teams.microsoft.com/sdk/v1.0/types/MicrosoftTeams.d.ts) to enable IntelliSense or similar support from your code editor as well as compile-type type checking as part of your build.

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

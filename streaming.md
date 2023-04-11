---
layout: page
title: "Video streaming"
permalink: /video-streaming/
layout: default
nav_order: 5
---

# Video Streaming

## Getting started
Your account must have video streaming enabled before you can use it. For help setting it up, please contact us at info@colourbox.com. You can [read more about video streaming from Skyfish here](https://www.skyfish.com/features/streaming).

## Generating a video streaming URL
Skyfish enables video streaming by creating a URL to the chosen video that may be embedded somewhere online. When Skyfish generates a video streaming URL, it encodes the video into in the [HTTP Live Streaming format](https://en.wikipedia.org/wiki/HTTP_Live_Streaming). To see if a file already has a video streaming URL, execute:

```json
GET /media/:media_id/metadata/stream_url
```

If the endpoint returns 404, no video streaming URL is available. This could mean either that a stream has never been requested, or that a stream has been requested but has not yet finished being processed. 

To request a video streaming URL:

```json
POST media/:media_id/stream
```

If the video is accepted for encoding, the API will return:
```json
{"Status":"Created"}
```

You should then query `/media/:media_id/metadata/stream_url` at a regular interval to see when the job is done. Once the video streaming URL is fully processed, the user requesting it receives an email with the URL, and the output of the endpoint changes from a 404 to output resembling:

```json
{
  "Stream": "https://video.skyfish.com/fde3e881-79d6-4efa-858e-380113476546/AppleHLS1/stream-1337-4242.m3u8"
}
```

This is a public URL, pointing to the "playlist" of the video you requested to stream. Any video player capable of playing HLS can use it to play the video. 

## Playing streamed videos
To play the videos, you need a video player with support for HLS. 

### Using the Skyfish player
The easiest way to play the videos is to use the Skyfish Video player. This is essentially a wrapped [Video.js](https://videojs.com/) player. It can be embedded on your website using an iframe. The player will also automatically add any subtitles you have added to the video via the Skyfish web interface. 

To generate the iframe HTML tag to embed the streamed video, execute:

```json
GET https://player.skyfish.com/iframe.json
```

It will return something resembling:
```json
{
  "iframe": "<iframe src=\"https://player.skyfish.com/?v={v}&media={media}\" width=\"560\" height=\"315\" style=\"border: 0\" title=\"Skyfish video\" allow=\"fullscreen; accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture\" allowfullscreen onLoad=\"!function(e){try{e.style.maxWidth='100%';var n=e.width/e.height;if(0<n){var t,d;(t=function(){var t,i=e.getBoundingClientRect();t=i.width<e.width?(i.width/n).toFixed(0)+'px':'none',d!==t&&(e.style.maxHeight=d=t)})(),window.addEventListener('resize',t)}}catch(t){console.log(t)}}(this)\"></iframe>",
  "fields": [
    "v",
    "media"
  ]
}
```

Note that it returns two fields. `iframe` is the embed code to use. `fields` is a list of placeholders that you need to replace in the embedding code's URL, in the iframe tag's src attribute. 

Currently, two placeholders are needed: `v`, which is the url to the `m3u8` file returned by the API, and `media`, which is the ID of the media in question. 

Here is an example of a completed embed code, ready to be inserted into an HTML page online:
```html
<iframe src="https://player.skyfish.com/?v=https://video.skyfish.com/fde3e88d-79d6-4efa-858e-380113476546/AppleHLS1/stream-449912-53872201.m3u8&media=53872201" width="560" height="315" style="border: 0" title="Skyfish video" allow="fullscreen; accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen onLoad="!function(e){try{e.style.maxWidth='100%';var n=e.width/e.height;if(0<n){var t,d;(t=function(){var t,i=e.getBoundingClientRect();t=i.width<e.width?(i.width/n).toFixed(0)+'px':'none',d!==t&&(e.style.maxHeight=d=t)})(),window.addEventListener('resize',t)}}catch(t){console.log(t)}}(this)"></iframe>
```

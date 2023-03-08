---
layout: page
title: "Video streaming"
permalink: /video-streaming/
layout: default
nav_order: 5
---

# Video Streaming

## Getting started
Video streaming needs to be enabled on your account before it can be used. Please contact us to help setting it up. You can read more about Video streaming [here](https://www.skyfish.com/features/streaming)

## Generating a video streaming URL
When generating a video streaming URL the system encodes the video into in the [HTTP Live Streaming format](https://en.wikipedia.org/wiki/HTTP_Live_Streaming). To see if a file already has a video streaming URL do:

```json
GET /media/:media_id/metadata/stream_url
```

If the endpoint returns 404 it means that no video streaming URL is available. It could either mean that a stream has never been requested, or that a stream has been requested but hasn't finished processing yet. 

To request a video streaming URL do:

```json
POST media/:media_id/stream
```

If the video is accepted for encoding it will return
```json
{"Status":"Created"}
```

You should then query `/media/:media_id/metadata/stream_url` at a regular interval to see when the job is done. Once the processing is done, the user requesting the stream  gets an email and the output of the endpoint changes from a 404 to:

```json
{
  "Stream": "https://video.skyfish.com/fde3e881-79d6-4efa-858e-380113476546/AppleHLS1/stream-1337-4242.m3u8"
}
```

This is a public URL pointing to the "playlist" of the video you requested. Any video player capable of playing HLS can use it play the video. 

## Playing the video
To play the video you need a video player with support for HLS. 

The easiest way is to use the Skyfish Video player (which is essentially a wrapped [Video.js](https://videojs.com/) player). It can be embedded on your website using an iframe. To generate the embed do the following:

```json
GET https://player.skyfish.com/iframe.json
```

It will return something like:
```json
{
  "iframe": "<iframe src=\"https://player.skyfish.com/?v={v}&media={media}\" width=\"560\" height=\"315\" style=\"border: 0\" title=\"Skyfish video\" allow=\"fullscreen; accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture\" allowfullscreen onLoad=\"!function(e){try{e.style.maxWidth='100%';var n=e.width/e.height;if(0<n){var t,d;(t=function(){var t,i=e.getBoundingClientRect();t=i.width<e.width?(i.width/n).toFixed(0)+'px':'none',d!==t&&(e.style.maxHeight=d=t)})(),window.addEventListener('resize',t)}}catch(t){console.log(t)}}(this)\"></iframe>",
  "fields": [
    "v",
    "media"
  ]
}
```

It returns two fields: `iframe` which is embed code to use, and `fields` which is a list of placeholders than you need to replace. 

Currently two placeholders are needed: `v` which is the url to the `m3u8` file returned by the API and `media` which is the ID of the media in question. 

An example of a complete embed:
```html
<iframe src="https://player.skyfish.com/?v=https://video.skyfish.com/fde3e88d-79d6-4efa-858e-380113476546/AppleHLS1/stream-449912-53872201.m3u8&media=53872201" width="560" height="315" style="border: 0" title="Skyfish video" allow="fullscreen; accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen onLoad="!function(e){try{e.style.maxWidth='100%';var n=e.width/e.height;if(0<n){var t,d;(t=function(){var t,i=e.getBoundingClientRect();t=i.width<e.width?(i.width/n).toFixed(0)+'px':'none',d!==t&&(e.style.maxHeight=d=t)})(),window.addEventListener('resize',t)}}catch(t){console.log(t)}}(this)"></iframe>
```


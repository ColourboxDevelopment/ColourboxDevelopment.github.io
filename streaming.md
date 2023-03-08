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

If the video is accepting for encoding it will return
```json
{"Status":"Created"}
```

You should then query `/media/:media_id/metadata/stream_url` at a regular interval to see when the job is done. Once the processing is done, the user requesting the stream  gets an email and the output of the endpoint changes from a 404 to:

```json
{
  "Stream": "https://video.skyfish.com/fde3e88d-79d6-4efa-858e-380113476546/AppleHLS1/stream-449912-53872201.m3u8"
}
```

This is a public URL pointing to the "playlist" of the video you requested. Any video player capable of playing HLS can use it play the video. 

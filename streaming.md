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


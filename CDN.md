---
layout: page
title: "CDN"
permalink: /cdn/
layout: default
---

# CDN

## Getting started
CDN needs to be configured be it can be used. Please have a look at this [Getting Started page](https://www.skyfish.com/)

## Using CDN from the API
The CDN feature is actually just a "magic" folder. When files are added to it, a CDN link will automatically be generated (and removed again if the file is deleted from the folder). 

The first step is to figure out the id of the CDN folder:
```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>" https://api.colourbox.com/cdn/<company-id>/company/aliases
```

It will give you something like:
```json
{
  "count": 1,
  "limit": 1,
  "aliases": [
    {
      "folder_id": 1337,
      "alias": "cdn-tutorial",
      "folder_name": "CDN"
    }
  ]
}
```

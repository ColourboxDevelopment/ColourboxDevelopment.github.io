---
layout: page
title: "CDN"
permalink: /cdn/
layout: default
---

# CDN

## Getting started
CDN needs to be configured be it can be used. Please have a look at this [Getting Started page](https://www.skyfish.com/help/cdn-links/set-up-domain)

## Using CDN from the API
The CDN feature is actually just a "magic" folder. When files are added to it, a CDN link will automatically be generated (and removed again if the file is deleted from the folder). 

The first step is to figure out the id of the CDN folder. Note that a user can be in more than one company, so remember to specify the correct`company-id`. If in doubt what your company ID is, you can reach out to us on api@colourbox.com
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

In this case the ID of the folder is `1337`. The `alias` is part of the URL of any links generated. In this case URLS will be on the format: `https://cdn.skyfish.com/cdn-tutorial/<filename>`

### Generating a CDN link
Generating a CDN link for a file is as easy as to simply add it to the CDN folder found in the previos section. Assume you want to generate a link for the media with `unique_media_id` of: `42424242`:

```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>" -XPOST https://api.colourbox.com/media/42424242/folder/1337
```

Once the file is placed in the folder, the file will be queued for CDN link creation. Often the link will be ready within 20 seconds. 

### Fetching the CDN link
All search endpoints supports asking for `cdn_urls` as a return value. To get the CDN link for a specific file, specify the `unique_media_id` as a search paraemter and request `cdn_urls` as a return values:

```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>" api.colourbox.com/search?unique_media_id=42424242&return_values=cdn_urls
```

Response:

```json
{
  "response": {
    "media": [
      {
        "cdn_urls": [
          "https://cdn.skyfish.com/cdn-tutorial/<id>"
        ]
      }
    ],
    "hits": 1
  },
  "media_count": 1,
  "media_offset": 0
}
```

**NOTE**
Because creating CDN links goes through our internal queuing system there can be some delay. In case `cdn_urls` is empty even though the file was placed in the CDN folder, wait a few seconds and try again.

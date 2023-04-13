---
layout: page
title: "Media"
permalink: /media/
layout: default
nav_order: 3
---

# Media files


## Media metadata in Skyfish
A media file in Skyfish contains 6 pieces of metadata. They are, by metadata field: 

1. title
2. byline
3. copyright
4. description
5. expiration_date
6. keywords

The 'byline' describes who the media's creator is, i.e. the photographer, videographer, or artist.

To update media file metadata, the user must have `WRITE` access to the file being updated, and the update requests must be [authenticated](/authentication/).

### Getting a media file's information
Here is an example request for information, and what it could return:
```json
GET /media/:id/
{
  "id": 42,
  "created": "2022-09-20 13:37:42",
  "type": "stock",
  "filename": "filename.jpg",
  "file_md5": "71c9ed575adf2e658f945c655b27e643",
  "uploaded_by": 623746,
  "metadata": {
    "editorial": false,
    "adult_content": false,
    "keywords": {
      "en": [
        "Tag1",
        "Tag2"
      ]
    },
    "title": {
      "en": "Sample title",
      "da": "Sample title",
      "de": "Sample title"
    },
    "description": {
      "en": "Sample description"
    },
    "iptc": {
      "ApplicationRecordVersion": 0,
      "By-line": "Bruce Wayne",
      "CopyrightNotice": "1989"
    },
    "geolocation": null,
    "camera_created": 1650385577
  }
}
```

Note that `keywords`, `title` and `description` are divided into languages. This is deliberate, to future-proof our API. For now, only English, with the language code `en`, should be used. This is the field that that Skyfish looks and searches for, currently. 

### Updating the media's title
```json
POST /media/:id/metadata/title
{"en":"The title of the image"}
```

The "en" key in the map is the language the title is posted in. Currently the
language used should always be `en`.

### Updating the media's byline
```json
POST /media/:id/metadata/byline
"The name of the e.g. photographer"
```

Where the body is a json encoded string.

### Updating the copyright information
```json
POST /media/:id/metadata/copyright
"All rights reserved, etc."
```

Here, the body is a json encoded string.

### Updating the media's description

```json
POST /media/:id/metadata/description
{"en":"A picture of a cat"}
```

### Updating the media's expiration date for use
To see the current expiration date:

```json
GET /media/:id/metadata/expiration_date
{ "expiration_date": "2024-06-15" }
```
To update the current expiration date:
```json
POST /media/:id/metadata/expiration_date
{"expiration_year": "2024", "expiration_month": "4"}
```

To remove the expiration date, including to 'un-expire' files that are already expired:
```json
DELETE /media/:id/metadata/expiration_date
```

### Updating keywords
Using delta updates is usually the easiest option.

You can add keywords with the following:
```json
PUT /media/:id/metadata/keyword
{"en": ["horse"]}
```
To delete keywords:
```json
DELETE /media/:id/metadata/keyword
{"en": ["horse"]}
```
To add or remove keywords from multiple media files, you can specify multiple files with:
```json
PUT|DELETE /media/1,2,3/metadata/keyword
{"en": ["horse"]}
```
To reset a file's keywords and replace them with a specified set:
```json
POST /media/:id/metadata/keywords
{"en": ["only","these","keywords"]}
```
To remove all keywords:
```json
DELETE /media/:id/metadata/keywords
```

## Media operations
Files can be moved or copied between different folders in Skyfish. Please note: when users 'copy' a file to a different folder, a new copy of the file is **NOT** created. Instead, Skyfish creates a new pointer to the file in the folder that the file is 'copied' into. Therefore, the same file may be in mulitple folders. 

### Move and copy
To copy a media file into a folder:
```json
POST /media/:media_id/folder/:folder_id
```

To move a media file into a folder:
```json
POST /media/:media_id/move
{"from": 1337, "to": 1338}
```
The above file-moving operation would move the media file from folder `1337` to `1338` 

### Deleting a file
To delete a file from a folder:
```json
DELETE /media/:id/folder/:folderId
```

This will immediately delete the file, permanently. You can also instead move the file to the trash can, which is what our web interface does when users delete files:
```json
POST /media/:id/folder/:folderId/move_to_trash
```

## Downloading a file
To download a file from Skyfish:
```
GET /media/:id/download_location
```

The output will look like this:
```json
{
  "url": "....",
  "internal_elb_url": "....",
  "method": "GET"
}
```

`internal_elb_url` is used internally by Skyfish and should therefore be ignored. `url` is a signed URL from where the file can be downloaded. Please note that the link will expire after a set time, so the file should be downloaded immediately. 



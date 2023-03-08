---
layout: page
title: "Media"
permalink: /media/
layout: default
nav_order: 4
---

# Media


## Information about a media 
A media in Skyfish contains 6 pieces of metadata: title, byline (photographer),
copyright, description, expiration_date and keywords.

To update metadata the requests must be [authenticated](/authentication/) and the user must have
`WRITE` access to the media being updated.

### Getting info for a media
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

You might notice that `keywords`, `title` and `description` are divded into languages. This is to future proof our API. As for now, only `en` should be used. This is the field that that the system looks/searches for. 

### Updating the title
```json
POST /media/:id/metadata/title
{"en":"The title of the image"}
```

The "en" key in the map is the language the title is posted in. Currently the
language used should always be en.

### Updating the byline / photographer
```json
POST /media/:id/metadata/byline
"name of the photographer"
```

Where the body is a json encoded string.

### Updating the copyright
```json
POST /media/:id/metadata/copyright
"All rights reserved"
```

Where the body is a json encoded string.

### Updating the description

```json
POST /media/:id/metadata/description
{"en":"desc lol"}
```

### Updating the expiration date
To see the current expiraiton date

```json
GET /media/:id/metadata/expiration_date
{ "expiration_date": "2023-06-15" }
```

```json
POST /media/:id/metadata/expiration_date
{"expiration_year": "2023", "expiration_month": "4"}
```

To remove the expiration date, i.e. to unexpire the file:
```json
DELETE /media/:id/metadata/expiration_date
```

### Updating keywords
For updating keywords, using delta updates is usually the easiest option.

You can add keywords to a media with the following:
```json
PUT /media/:id/metadata/keyword
{"en": ["horse"]}
```

To delete them you do:

```json
DELETE /media/:id/metadata/keyword
{"en": ["horse"]}
```

If you need to add / remove keywords from multiple media you can specify multiple medias with:
```json
PUT|DELETE /media/1,2,3/metadata/keyword
{"en": ["horse"]}
```
You can also set exactly the keywords you want the media to have with:
```json
POST /media/:id/metadata/keywords
{"en": ["only","these"]}
```

Removing all keywords from a media can be done with:
```json
DELETE /media/:id/metadata/keywords
```

## Media operations
A file can be copied and moved around between different folders. Please note that when we say we copy a file to a different folder, a new copy of the file is **NOT** created. Instead, a pointer to the file is created in the folder it is copied into. in other words, the same file can be in mulitple folders. 

### Move and copy
To copy a media into a folder do:
```json
POST /media/:media_id/folder/:folder_id
```

To move a media into a folder do:
```json
POST /media/:media_id/move
{"from": 1337, "to": 1338}
```
The above move operation would move the media from folder `1337` to `1338` 

### Deleting a file
To delete a file from a folder you do:
```json
DELETE /media/:id/folder/:folderId
```

This will immediately delete the file. You can also chose to move it to the trash can instead (this is what our web interface do when deleting a file):
```json
POST /media/:id/folder/:folderId/move_to_trash
```

## Downloading a file





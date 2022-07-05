---
layout: page
title: "Updating media metadata"
permalink: /media-metadata-update/
layout: default
nav_order: 4
---

# Media metadata update
A media in Skyfish contains 6 pieces of metadata: title, byline (photographer),
copyright, description, expiration_date and keywords.

To update metadata the requests must be [authenticated](/authentication/) and the user must have
WRITE access to the media being update.

In general updates works by using the http verbs with a json encoded body.

## Updating the title

```json
POST /media/:id/metadata/title
{"en":"The title of the image"}
```

the "en" key in the map is the language the title is postet in. Currently the
language used should always be en.

## Updating the byline / photographer
```json
POST /media/:id/metadata/byline
"name of the photographer"
```
Where the body is a json encoded string.

## Updating the copyright
```json
POST /media/:id/metadata/copyright
"All rights reserved"
```
Where the body is a json encoded string.

## Updating the description

```json
POST /media/:id/metadata/description
{"en":"desc lol"}
```

## Updating the expiration date
```json
POST /media/:id/metadata/expiration_date
{"expiration_year": "2023", "expiration_month": "4"}
```

## Updating keywords
For updating keywords, using delta updates is usually the easiest option.

You can add keywords to a media by PUTing them:
```json
PUT /media/:id/metadata/keyword
{"en": ["horse"]}
```

You can DELETE them:

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

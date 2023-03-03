---
layout: page
title: "Folder"
permalink: /folder/
layout: default
nav_order: 2
---

# Folder
The primary way of organizing media in the Skyfish API is through folders. Folders can easily be handled and modified through the Skyfish API.

To list all folders in your company do:
```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>" https://api.colourbox.com/folder
```
The output is on the form:
```
[
  {
    "id": 1891723,
    "name": "Picture of cats,
    "parent": null,
    "cover_photo_id": null,
    "company_id": 445605,
    "created": "2021-03-09 13:37:42",
    "permissions": {
      "groups": [
        "READ"
      ]
    }
  },
  {
    "id": 2020441,
    "name": "More pictures of cats",
    "parent": null,
    "cover_photo_id": null,
    "company_id": 445605,
    "created": "2022-01-25 08:34:57",
    "permissions": {
      "groups": [
        "READ",
        "ADMIN",
        "WRITE"
      ]
    }
  }
]  
```

The endpoint supports the following filtering options (all optional):
| Parameter        | Description         
| ------------- |-------------
| q    | Query string matching against the folder name 
| id    | Limit the list to the folder that has the given ID. Multiple IDs can be given seperated by "+"
| parent    | Limit the list to folders with the parent ID specified
| sort_by    | Specifies which field you would like to sort by, currently `name` and `created` are supported.
| sort_order    | Can be either `asc`, or `desc`. Default is `asc`



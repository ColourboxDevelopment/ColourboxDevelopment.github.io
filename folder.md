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

The `permissions` key specify the access right your user has

| name        | Description         
| ------------- |-------------
| READ    | User can see the folder
| ADMIN    | User can change the folder
| WRITE    | Limit the list to folders with the parent ID specified
| PERMANENT    | The folder is permanent (cannot be altered or deleted)
| TRASH    | 	The folder is the user’s trashcan (implies PERMANENT)
| PUBLIC | 	The folder is a public folder (implies PERMANENT)



The endpoint supports the following filtering options (all optional):

| Parameter        | Description         
| ------------- |-------------
| q    | Query string matching against the folder name 
| id    | Limit the list to the folder that has the given ID. Multiple IDs can be given seperated by "+"
| parent    | Limit the list to folders with the parent ID specified
| sort_by    | Specifies which field you would like to sort by, currently `name` and `created` are supported.
| sort_order    | Can be either `asc`, or `desc`. Default is `asc`

## Adding tags
You can add multiple tags on a folder. When a tag is added to a folder, it is automatically inherited down to all the files that are located in the folder (and any subfolder underneath). When a file is moved into a folder where a folder tag set, that file automatically gets it. Similar, if the file is moved out of the folder, the tag will be removed again. 

You modify folder tags in the following way:


List all folder tags
```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>"  https://api.colourbox.com/folder/:id/tags
```

Add folder tags `tag1` and `tag2`
```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>"  -XPUT https://api.colourbox.com/folder/:id/tags -d'["tag1", "tag2"]'
```

Delete folder tags `tag1` and `tag2`
```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>"  -XDELETE https://api.colourbox.com/folder/:id/tags -d'["tag1", "tag2"]'
```

Delete all folder tags
```
curl -H "Authorization: CBX-SIMPLE-TOKEN Token=<token>"  -XDELETE https://api.colourbox.com/folder/:id/tags/purge
```


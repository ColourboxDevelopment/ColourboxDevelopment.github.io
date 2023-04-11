---
layout: page
title: "Folder"
permalink: /folder/
layout: default
nav_order: 2
---

# Folders in Skyfish
Folders are the primary way to organize media in Skyfish. Folders can easily be handled and modified through the Skyfish API.


### How to fetch folders via the API
To list all the Skyfish folders that exist in your company, execute:
```
GET https://api.colourbox.com/folder
```
The output will resemble something like the following:
```json
[
  {
    "id": 1891723,
    "name": "Picture of cats",
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

The `permissions` key specifies the access rights that your users have:

| Name        | Description         
| ------------- |-------------
| READ    | User can see the folder.
| ADMIN    | User can change the folder.
| WRITE    | Limit the list to folders with the parent ID specified. [Is this correct?]
| PERMANENT    | The folder is permanent (it cannot be altered or deleted).
| TRASH    | 	The folder is the user’s trashcan (implies it is PERMANENT).
| PUBLIC | 	The folder is a Public Media Gallery folder (implies it is PERMANENT).



This endpoint supports the following filtering options (all optional):

| Parameter        | Description         
| ------------- |-------------
| q    | Query the string matching the folder name. 
| id    | Limit the returned list to the folder that has the specified ID. Multiple IDs can be specified, seperated by "+".
| parent    | Limit the list to folders with the parent folder ID specified.
| sort_by    | Specifies which field you would like to sort by. Currently, `name` and `created` are supported.
| sort_order    | Can be either `asc` or `desc`. Default is `asc`.


### Creating a folder
To create a folder, make a `POST` request to `https://api.colourbox.com/folder/`

In the request's body, you must specify the following:

| Parameter        | Description         
| ------------- |-------------
| name    | The name of the folder.
| parent    | The id of the parent folder. To make your created folder a root folder, use `null` .


Similar, to update a folder, make a `POST` request to `https://api.colourbox.com/folder/:id/`. The structure of the request's body is the same as when creating a folder.

Some examples follow. Assume for all of them that the folder ID is `110917` and the parent folder ID is `110220`. 

To rename the folder to `Testing`, execute:
```json
POST https://api.colourbox.com/folder/110917
{"name": "Testing", "parent": 110220}
```

To move the folder, so that it becomes a root folder:
```json
POST https://api.colourbox.com/folder/110917
{"name": "Testing", "parent": null}
```

### Deleting a folder
To delete a folder, execute the following:
```json
DELETE https://api.colourbox.com/folder/:id
```

To empty a folder, i.e. to delete all contained files and subfolders but keep the folder itself:
```json
POST  https://api.colourbox.com/folder/:id/purge
```

When deleting a folder via the Skyfish web UI, the folder is not deleted permanently. Instead, it is moved to the trashcan. Items in the trashcan for 30 days get permanently deleted.

To move a folder to the trashcan via the API:
```json
POST https://api.colourbox.com/folder/:id/move_to_trash
```


### Adding tags to folders
Folders may have multiple tags added to them. When a tag is added to a folder, the tag is automatically inherited by all the files that are located in the folder, and in any subfolder therein. When a file is moved into a folder that already has a tag added to it, that file automatically inherits the tag, too. Similarly, if the file is moved out of the folder, the inherited tag will be removed from the file. 

Modify these 'Folder Tags' in the following way:


List all Folder Tags:
```json
GET https://api.colourbox.com/folder/:id/tags
```

Add tags `tag1` and `tag2` to a folder:
```json
PUT https://api.colourbox.com/folder/:id/tags
["tag1", "tag2"]
```

Delete the Folder Tags `tag1` and `tag2`:
```json
DELETE https://api.colourbox.com/folder/:id/tags
["tag1", "tag2"]
```

Delete all Folder Tags:
```json
DELETE https://api.colourbox.com/folder/:id/tags/purge
```

### Downloading an entire folder as a .zip file
You can download a folder via the API. First, make a request to queue the folder for zipping. The API will return a job ID that can be queried for a job status report. Once the folder is zipped, you will receive a signed URL to download the .zip from. The user requesting the download will also get an email that contains the download URL. 

To queue a folder for zipping:
```json
POST https://api.colourbox.com/folder/:id/zip-file
```

You will receive something resembling the following:
```json
{
  "job_id":"9xahozLZnDp3JFcqBmbYQysIVrj6Ni",
  "state":"PENDING",
  "url":null,
  "valid_until":null
}
```

| Key        | Description         
| ------------- |-------------
| job_id    | The ID of the zipping job. Use this to query the job status.
| state    | The current status of the zipping job.
| url    | The signed URL to download the .zip from. Until the zip job is done, this field will be `null`.
| valid_until    | A unix timestamp dictating when the above URL expires. Until the zip job is done, this field will be `null`.

To query the status of the zip job:
```json
POST https://api.colourbox.com/zip-file/:job_id
```

The output is the same format as: ```https://api.colourbox.com/folder/:id/zip-file```

When the job is done, you will receive output that resembles:
```json
{
  "job_id":"9xahozLZnDp3JFcqBmbYQysIVrj6Ni",
  "state":"DONE",
  "url":"https:\/\/colourbox-zipfiles-prod.s3.eu-west-1.amazonaws.com\/tmp6yjgtuus.zip?.....",
  "valid_until":1678233600
}
```

Please note: We cache zip creation. Therefore, if you request a .zip file for a folder, but there already exists a non-expired/non-invalidated .zip file, we will return the existing .zip file. This means that your call to `https://api.colourbox.com/folder/:id/zip-file` may return a download URL immediately. 
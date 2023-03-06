---
layout: page
title: "Folder"
permalink: /folder/
layout: default
nav_order: 2
---

# Folder
The primary way of organizing media in the Skyfish API is through folders. Folders can easily be handled and modified through the Skyfish API.


### Fetching folders
To list all folders in your company do:
```
GET https://api.colourbox.com/folder
```
The output is on the form:
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


### Creating a folder
To create a folder you make a `POST` request to `https://api.colourbox.com/folder/`

In the body you need to specify the following

| Parameter        | Description         
| ------------- |-------------
| name    | Name of the folder
| parent    | The id of the parent folder. Specify `null` to make it a root folder


Similar, to update a folder you make a `POST` request to `https://api.colourbox.com/folder/:id/`. The structure of the body is the same as when creating


For the following examples assume the id of the folder is `110917` and the parent is `110220`. 

To rename the folder to `Testing` do
```json
POST https://api.colourbox.com/folder/110917
{"name": "Testing", "parent": 110220}
```

Move the folder so it becomes a root folder
```json
POST https://api.colourbox.com/folder/110917
{"name": "Testing", "parent": null}
```

### Deleting a folder
To delete a folder do the following
```json
DELETE https://api.colourbox.com/folder/:id'
```

To empty a folder, i.e., delete all files and subfolders but keep the folder itself do:
```json
POST  https://api.colourbox.com/folder/:id/purge'
```

When deleting a folder via our web interface, the folder it not deleted right away. Instead it is moved to the trashcan (which is automatically emptied after 30 days). 
To do that via the API do:
```json
POST https://api.colourbox.com/folder/:id/move_to_trash'
```


### Adding tags
You can add multiple tags on a folder. When a tag is added to a folder, it is automatically inherited down to all the files that are located in the folder (and any subfolder underneath). When a file is moved into a folder where a folder tag set, that file automatically gets it. Similar, if the file is moved out of the folder, the tag will be removed again. 

You modify folder tags in the following way:


List all folder tags
```json
GET https://api.colourbox.com/folder/:id/tags
```

Add folder tags `tag1` and `tag2`
```
PUT https://api.colourbox.com/folder/:id/tags
["tag1", "tag2"]
```

Delete folder tags `tag1` and `tag2`
```json
DELETE https://api.colourbox.com/folder/:id/tags
["tag1", "tag2"]
```

Delete all folder tags
```json
DELETE https://api.colourbox.com/folder/:id/tags/purge
```

### Downloading a folder as a zip file
It is possible to download a folder via the API. To download a folder you first make a request to queue the folder for zipping. The API will give back a job ID that can be queried for status. Once the zip generation is done, you will get back a signed URL from where you can download. The user requesting the download will also get an email with the link. 

To queue a folder for zipping do:
```json
POST https://api.colourbox.com/folder/:id/zip-file
```

You will get the following back
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
| job_id    | The ID of the zip job generating. Use this to query the stauts
| state    | The current status of the job
| url    | Signed URL from where the zip can be downloaded from. When the zip job is not done this field will be `null`
| valid_until    | A unix timestamp dictating when the above URL expires. When the zip job is not done this field will be `null`

To query the status of the job do the following:
```json
POST https://api.colourbox.com/zip-file/:job_id
```

The output is the same format as ```https://api.colourbox.com/folder/:id/zip-file```

Once the job is done you will get something like:
```json
{
  "job_id":"9xahozLZnDp3JFcqBmbYQysIVrj6Ni",
  "state":"DONE",
  "url":"https:\/\/colourbox-zipfiles-prod.s3.eu-west-1.amazonaws.com\/tmp6yjgtuus.zip?.....",
  "valid_until":1678233600
}
```

Please note that we cache zip creation. So if you request a zip file  for a folder where there already exist a non-expired/invalidated zip file, we will return that zile back immediately. This means that the call to `https://api.colourbox.com/folder/:id/zip-file` might return back a URL right away. 

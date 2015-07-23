## Assets
The assets endpoint allow access to obtain 

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$auth     = ApiAuth::initiate($settings);
$apiUrl   = "https://your-mautic.com"; 
$assetApi = MauticApi::getContext("assets", $auth, $apiUrl);
```

### Get Asset
```php
<?php

//...
$asset = $assetApi->get($id);
```
```json
{
    "asset": {
        "id": 1,
        "title": "Product Whitepaper",
        "description": "Some description",
        "alias": "whitepaper",
        "language": "en",
        "isPublished": true,
        "publishUp": "2015-06-07T06:28:27+00:00",
        "publishDown": "2015-06-30T06:28:27+00:00",
        "dateAdded": "2015-06-07T06:28:27+00:00",
        "createdBy": 1,
        "createdByUser": "Joe Smith",
        "dateModified": "2015-06-010T09:30:47+00:00",
        "modifiedBy": 1,
        "modifiedByUser": "Joe Smith",
        "downloadCount": 10,
        "uniqueDownloadCount": 8,
        "revision": 1,
        "category": "Whitepapers",
        "extension": "pdf",
        "mime": "application/pdf",
        "size": 269128,
        "downloadUrl": "https://your-mautic.com/asset/1:whitepaper"
    }
}
```
Get an individual asset by ID.

#### HTTP Request

`GET /assets/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Asset Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the asset
title|string|Title/name of the asset
description|string/null|Description of the asset
alias|string|Used to generate the URL for the asset
language|string|Locale of the asset
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the asset should be published
publishDown|datetime/null|Date/time the asset should be un published
dateAdded|datetime|Date/time asset was created
createdBy|int|ID of the user that created the asset
createdByUser|string|Name of the user that created the asset
dateModified|datetime/null|Date/time asset was last modified
modifiedBy|int|ID of the user that last modified the asset
modifiedByUser|string|Name of the user that last modified the asset
downloadCount|int|Total number of downloads
uniqueDownloadCount|int|Unique number of downloads
revision|int|Revision version
category|string/null|Name of the category
extension|string|Extension of the asset
mime|string|Mime type of the asset
size|int|Filesize of the asset in bytes
downloadUrl|string|Public download URL for the asset

### List Assets
```php
<?php
// ...

$assets = $assetApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir);
```
```json
{
    "total": 1,
    "assets": [
        {
            "id": 1,
            "title": "Product Whitepaper",
            "description": "Some description",
            "alias": "whitepaper",
            "language": "en",
            "isPublished": true,
            "publishUp": "2015-06-07T06:28:27+00:00",
            "publishDown": "2015-06-30T06:28:27+00:00",
            "dateAdded": "2015-06-07T06:28:27+00:00",
            "createdBy": 1,
            "createdByUser": "Joe Smith",
            "dateModified": "2015-06-010T09:30:47+00:00",
            "modifiedBy": 1,
            "modifiedByUser": "Joe Smith",
            "downloadCount": 10,
            "uniqueDownloadCount": 8,
            "revision": 1,
            "category": "Whitepapers",
            "extension": "pdf",
            "mime": "application/pdf",
            "size": 269128,
            "downloadUrl": "https://your-mautic.com/asset/1:whitepaper"
        }
    ]
}
```
#### HTTP Request

`GET /assets`

**Query Parameters**

Name|Description
----|-----------
search|String or search command to filter entities by.
start|Starting row for the entities returned. Defaults to 0.
limit|Limit number of entities to return. Defaults to the system configuration for pagination (30).
orderBy|Column to sort by. Can use any column listed in the response.
orderByDir|Sort direction: asc or desc.
publishedOnly|Only return currently published entities.

#### Response

`Expected Response Code: 200`

See JSON code example.

**Properties**

Same as [Get Asset](#get-asset).
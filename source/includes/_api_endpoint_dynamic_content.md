## Dynamic Content
Use this endpoint to obtain details on Mautic's web dynamic content.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$auth    = ApiAuth::initiate($settings);
$apiUrl  = "https://your-mautic.com"; 
$dynamicConentApi = MauticApi::getContext("dynamicConents", $auth, $apiUrl);
```

### Get Dynamic Conent
```php
<?php

//...
$dynamicConent = $dynamicConentApi->get($id);
```
```json
{  
    "dynamicContent":{  
        "isPublished":true,
        "dateAdded":"2016-06-20T11:26:51+00:00",
        "createdBy":1,
        "createdByUser":"John Doe",
        "dateModified":"2016-08-08T16:36:27+00:00",
        "modifiedBy":1,
        "modifiedByUser":"John Doe",
        "id":1,
        "name":"DC13",
        "category":null,
        "publishUp":null,
        "publishDown":null,
        "sentCount":0,
        "variantParent":null,
        "variantChildren":[]
    }
}
```
Get an individual dynamicConent by ID.

#### HTTP Request

`GET /dynamicconents/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Dynamic Conent Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the dynamic conent
name|string|Name of the dynamic conent
description|string/null|Description of the dynamic conent
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the dynamic conent should be published
publishDown|datetime/null|Date/time the dynamic conent should be un published
dateAdded|datetime|Date/time dynamic conent was created
createdBy|int|ID of the user that created the dynamic conent
createdByUser|string|Name of the user that created the dynamic conent
dateModified|datetime/null|Date/time dynamic conent was last modified
modifiedBy|int|ID of the user that last modified the dynamic conent
modifiedByUser|string|Name of the user that last modified the dynamic conent
variantChildren|array|Array of Dynamic Conent entities for variants of this landing dynamic conent
variantParent|object|The parent/main dynamic conent if this is a variant (A/B test)
sentCount|int|Count of how many times the dynamic content was sent

### List Dynamic Conents
```php
<?php
// ...

$dynamicConents = $dynamicConentApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir);
```
```json
{  
    "total":30,
    "dynamicContents":[  
        {  
            "isPublished":true,
            "dateAdded":"2016-06-20T11:27:09+00:00",
            "createdBy":1,
            "createdByUser":"John Doe",
            "dateModified":"2016-08-22T17:14:01+00:00",
            "modifiedBy":1,
            "modifiedByUser":"John Doe",
            "id":2,
            "name":"CD2",
            "category":null,
            "publishUp":null,
            "publishDown":null,
            "sentCount":0,
            "variantParent":null,
            "variantChildren":[]
        }
    ]
}
```
#### HTTP Request

`GET /dynamicconents`

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

Same as [Get Dynamic Conent](#get-dynamic-conent).

### Create Dynamic Conent
```php
<?php 

$data = array(
    'name'        => 'Dynamic Conent A',
    'isPublished' => 1
);

$dynamicConent = $dynamicConentApi->create($data);
```
Create a new dynamicConent.

#### HTTP Request

`POST /dynamicConent/new`

**Post Parameters**

Name|Type|Description
----|----|-----------
id|int|ID of the dynamic conent
name|string|Name of the dynamic conent
description|string/null|Description of the dynamic conent
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the dynamic conent should be published
publishDown|datetime/null|Date/time the dynamic conent should be un published
dateAdded|datetime|Date/time dynamic conent was created
createdBy|int|ID of the user that created the dynamic conent
createdByUser|string|Name of the user that created the dynamic conent
dateModified|datetime/null|Date/time dynamic conent was last modified
modifiedBy|int|ID of the user that last modified the dynamic conent
modifiedByUser|string|Name of the user that last modified the dynamic conent
variantChildren|array|Array of Dynamic Conent entities for variants of this landing dynamic conent
variantParent|object|The parent/main dynamic conent if this is a variant (A/B test)
sentCount|int|Count of how many times the dynamic content was sent

#### Response

`Expected Response Code: 201`

**Properties**

Same as [Get Dynamic Conent](#get-dynamic-conent).

### Edit Dynamic Conent
```php
<?php

$id   = 1;
$data = array(
    'name'        => 'New dynamicConent name',
    'isPublished' => 0
);

// Create new a dynamicConent of ID 1 is not found?
$createIfNotFound = true;

$dynamicConent = $dynamicConentApi->edit($id, $data, $createIfNotFound);
```
Edit a new dynamicConent. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a dynamicConent if the given ID does not exist and clears all the dynamic conent information, adds the information from the request.
**PATCH** fails if the dynamic conent with the given ID does not exist and updates the dynamic conent field values with the values form the request.

#### HTTP Request

To edit a dynamicConent and return a 404 if the dynamic conent is not found:

`PATCH /dynamicconents/ID/edit`

To edit a dynamicConent and create a new one if the dynamic conent is not found:

`PUT /dynamicconents/ID/edit`

**Post Parameters**

Name|Type|Description
----|----|-----------
id|int|ID of the dynamic conent
name|string|Name of the dynamic conent
description|string/null|Description of the dynamic conent
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the dynamic conent should be published
publishDown|datetime/null|Date/time the dynamic conent should be un published
dateAdded|datetime|Date/time dynamic conent was created
createdBy|int|ID of the user that created the dynamic conent
createdByUser|string|Name of the user that created the dynamic conent
dateModified|datetime/null|Date/time dynamic conent was last modified
modifiedBy|int|ID of the user that last modified the dynamic conent
modifiedByUser|string|Name of the user that last modified the dynamic conent
variantChildren|array|Array of Dynamic Conent entities for variants of this landing dynamic conent
variantParent|object|The parent/main dynamic conent if this is a variant (A/B test)
sentCount|int|Count of how many times the dynamic content was sent

#### Response

If `PUT`, the expected response code is `200` if the dynamic conent was edited or `201` if created.

If `PATCH`, the expected response code is `200`.

**Properties**

Same as [Get Dynamic Conent](#get-dynamic-conent).

### Delete Dynamic Conent
```php
<?php

$dynamicConent = $dynamicConentApi->delete($id);
```
Delete a dynamicConent.

#### HTTP Request

`DELETE /dynamicconents/ID/delete`

#### Response

`Expected Response Code: 200`

**Properties**

Same as [Get Dynamic Conent](#get-dynamic-conent).

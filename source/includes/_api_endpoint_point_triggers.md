## Point Triggers
Use this endpoint to obtain details on Mautic's point triggers.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$auth     = ApiAuth::initiate($settings);
$apiUrl   = "https://your-mautic.com"; 
$pointApi = MauticApi::getContext("pointTriggers", $auth, $apiUrl);
```

### Get Point Trigger
```php
<?php

//...
$trigger = $pointApi->get($id);
```
```json
{
    "trigger": {
         "id": 1,
         "name": "Trigger test",
         "description": null,
         "category": null,      
         "isPublished": true,      
         "publishUp": null,
         "publishDown": null,
         "dateAdded": "2015-07-23T03:20:42-05:00",
         "createdBy": 1,
         "createdByUser": "Joe Smith",
         "dateModified": null,
         "modifiedBy": null,
         "modifiedByUser": null,l,
         "points": 10,
         "color": "ab5959",
         "events": {
             "1": {
                 "id": 1,
                 "type": "email.send",
                 "name": "Send email",
                 "description": null,
                 "order": 1,
                 "properties": {
                    "email": 21
                 }
             }
         }
     }
}
```
Get an individual point trigger by ID.

#### HTTP Request

`GET /points/triggers/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Point Trigger Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the point
name|string|Name of the point
description|string/null|Description of the point
category|string|Category name
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the point should be published
publishDown|datetime/null|Date/time the point should be un published
dateAdded|datetime|Date/time point was created
createdBy|int|ID of the user that created the point
createdByUser|string|Name of the user that created the point
dateModified|datetime/null|Date/time point was last modified
modifiedBy|int|ID of the user that last modified the point
modifiedByUser|string|Name of the user that last modified the point
points|int|The minimum number of points before the trigger events are executed
color|string|Color hex to highlight the lead with. This value does NOT include the pound sign (#)
events|array|Array of TriggerEvent entities for this trigger. See below.

**Trigger Event Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the event
type|string|Event type
name|string|Name of the event
description|string|Description of the event
order|int|Event order
properties|array|Configured properties for the event

### List Point Triggers

```php
<?php
// ...

$triggers = $pointApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir);
```
```json
{
    "total": 1,
    "triggers": [
        {
            "id": 1,
            "name": "Trigger test",
            "description": null,
            "category": null,      
            "isPublished": true,      
            "publishUp": null,
            "publishDown": null,
            "dateAdded": "2015-07-23T03:20:42-05:00",
            "createdBy": 1,
            "createdByUser": "Joe Smith",
            "dateModified": null,
            "modifiedBy": null,
            "modifiedByUser": null,l,
            "points": 10,
            "color": "ab5959",
            "events": {
                "1": {
                    "id": 1,
                    "type": "email.send",
                    "name": "Send email",
                    "description": null,
                    "order": 1,
                    "properties": {
                        "email": 21
                    }
                }
            }
        }
    ]
}
```
#### HTTP Request

`GET /points/triggers`

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

Same as [Get Point Trigger](#get-point-trigger).
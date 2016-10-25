## Segments
Use this endpoint to obtain details on Mautic's contact segments or to manipulate contact memberships.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$auth     = ApiAuth::initiate($settings);
$apiUrl   = "https://your-mautic.com"; 
$segmentApi = MauticApi::getContext("segments", $auth, $apiUrl);
```

### Get Segment
```php
<?php

//...
$segment = $segmentApi->get($id);
```
```json
    "list": {
        "id": 47,
        "isPublished": 1,
        "dateAdded": "2015-07-21T12:27:12-05:00",
        "createdBy": 1,
        "createdByUser": "Joe Smith",
        "dateModified": "2015-07-21T14:12:03-05:00",
        "modifiedBy": 1,
        "modifiedByUser": "Joe Smith",
        "name": "Segment A",
        "alias": "segment-a",
        "description": "This is my first segment created via API.",
        "filters": [
          "glue": "and",
          "field": "city",
          "type": "text",
          "filter": "Prague",
          "display": null,
          "operator": "=",
        ],
        "isGlobal": true
    }
```
Get an individual segment by ID.

#### HTTP Request

`GET /segments/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Segment Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the segment
isPublished|boolean|Whether the segment is published
dateAdded|datetime|Date/time segment was created
createdBy|int|ID of the user that created the segment
createdByUser|string|Name of the user that created the segment
dateModified|datetime/null|Date/time segment was last modified
modifiedBy|int|ID of the user that last modified the segment
modifiedByUser|string|Name of the user that last modified the segment
name|string|Segment name
alias|string|Segment alias
description|string|Segment description
filters|array|Smart filters for the segment
isGlobal|boolean|Whether the segment is global. 0 means only the author will see it.


### List Contact Segments

```php
<?php

//...
$segments = $segmentApi->getList();
```
```json
{
  "total": 13,
  "lists": [
    {
        "id": 47,
        "isPublished": 1,
        "dateAdded": "2015-07-21T12:27:12-05:00",
        "createdBy": 1,
        "createdByUser": "Joe Smith",
        "dateModified": "2015-07-21T14:12:03-05:00",
        "modifiedBy": 1,
        "modifiedByUser": "Joe Smith",
        "name": "Segment A",
        "alias": "segment-a",
        "description": "This is my first segment created via API.",
        "filters": [
          "glue": "and",
          "field": "city",
          "type": "text",
          "filter": "Prague",
          "display": null,
          "operator": "=",
        ],
        "isGlobal": true
    },
    ...
  ]
}
```
Returns a list of contact segments available to the user. This list is not filterable.

#### HTTP Request

`GET /segments`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Segment Properties**

Name|Type|Description
----|----|-----------
total|int|Count of all segments
id|int|ID of the segment
isPublished|boolean|Whether the segment is published
dateAdded|datetime|Date/time segment was created
createdBy|int|ID of the user that created the segment
createdByUser|string|Name of the user that created the segment
dateModified|datetime/null|Date/time segment was last modified
modifiedBy|int|ID of the user that last modified the segment
modifiedByUser|string|Name of the user that last modified the segment
name|string|Segment name
alias|string|Segment alias
description|string|Segment description
filters|array|Smart filters for the segment
isGlobal|boolean|Whether the segment is global. 0 means only the author will see it.

### Create Segment
```php
<?php 

$data = array(
    'name'        => 'Segment A',
    'alias'       => 'segment-a',
    'description' => 'This is my first segment created via API.',
    'isPublished' => 1
);

$segment = $segmentApi->create($data);
```
Create a new segment.

#### HTTP Request

`POST /segments/new`

**Post Parameters**

Name|Description
----|-----------
name|Segment name is the only required field
alias|Name alias generated automatically if not set
description|A description of the segment.
isPublished|A value of 0 or 1
isGlobal|boolean|Whether the segment is global. 0 means only the author will see it.

#### Response

`Expected Response Code: 201`

**Properties**

Same as [Get Segment](#get-segment).

### Edit Segment
```php
<?php

$id   = 1;
$data = array(
    'name'        => 'New segment name',
    'isPublished' => 0
);

// Create new a segment of ID 1 is not found?
$createIfNotFound = true;

$segment = $segmentApi->edit($id, $data, $createIfNotFound);
```
Edit a new segment. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a segment if the given ID does not exist and clears all the segment information, adds the information from the request.
**PATCH** fails if the segment with the given ID does not exist and updates the segment field values with the values form the request.

#### HTTP Request

To edit a segment and return a 404 if the segment is not found:

`PATCH /segments/ID/edit`

To edit a segment and create a new one if the segment is not found:

`PUT /segments/ID/edit`

**Post Parameters**

Name|Description
----|-----------
name|Segment name is the only required field
alias|Name alias generated automatically if not set
description|A description of the segment.
isPublished|A value of 0 or 1
isGlobal|boolean|Whether the segment is global. 0 means only the author will see it.

#### Response

If `PUT`, the expected response code is `200` if the segment was edited or `201` if created.

If `PATCH`, the expected response code is `200`.

**Properties**

Same as [Get Segment](#get-segment).

### Delete Segment
```php
<?php

$segment = $segmentApi->delete($id);
```
Delete a segment.

#### HTTP Request

`DELETE /segments/ID/delete`

#### Response

`Expected Response Code: 200`

**Properties**

Same as [Get Segment](#get-segment).


### Add Contact to a Segment

```php
<?php

//...
$response = $segmentApi->addContact($segmentId, $contactId);
if (!isset($response['success'])) {
    // handle error
}
```
```json
{
    "success": true
}
```

Manually add a contact to a specific segment.

#### HTTP Request

`POST /segments/SEGMENT_ID/contact/add/CONTACT_ID`

#### Response

`Expected Response Code: 200`

See JSON code example.


### Remove Contact from a Segment

```php
<?php

//...
$response = $segmentApi->removeContact($segmentId, $contactId);
if (!isset($response['success'])) {
    // handle error
}
```
```json
{
    "success": true
}
```

Manually remove a contact to a specific segment.

#### HTTP Request

`POST /segments/SEGMENT_ID/contact/remove/CONTACT_ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

## Forms
```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$auth    = ApiAuth::initiate($settings);
$apiUrl  = "https://your-mautic.com"; 
$formApi = MauticApi::getContext("forms", $auth, $apiUrl);
```

### Get Form
```php
<?php

//...
$form = $formApi->get($id);
```
```json
{
    "form": {
        "id": 3,
        "name": "Newlsetter",
        "alias": "newsletter",
        "description": null,
        "isPublished": true,
        "publishUp": null,
        "publishDown": null,
        "dateAdded": "2015-07-15T15:06:02-05:00",
        "createdBy": 1,
        "createdByUser": "Joe Smith",
        "dateModified": "2015-07-20T13:11:56-05:00",
        "modifiedBy": 1,
        "modifiedByUser": "Joe Smith",
        "category": null,
        "cachedHtml": "\n\n<script...",
        "template": null,
        "submissionCount": 10,
        "fields": {
            "26": {
                "id": 26,
                "label": "Email",
                "showLabel": false,
                "alias": "email",
                "type": "text",
                "defaultValue": null,
                "isRequired": true,
                "validationMessage": "Email is required",
                "helpMessage": null,
                "order": 1,
                "properties": {
                    "placeholder": "Email address"
                },
                "labelAttributes": null,
                "inputAttributes": null,
                "containerAttributes": null
            },
            "27": {
                "id": 27,
                "label": "Submit",
                "showLabel": true,
                "alias": "submit",
                "type": "button",
                "defaultValue": null,
                "isRequired": false,
                "validationMessage": null,
                "helpMessage": null,
                "order": 4,
                "properties": [],
                "labelAttributes": null,
                "inputAttributes": null,
                "containerAttributes": null
            }
        },
        "actions": {
            "4": {
                "id": 4,
                "type": "email.send.lead",
                "name": "Send thank you email",
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
Get an individual form by ID.

#### HTTP Request

`GET /forms/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Form Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the form
name|string|Name of the form
description|string/null|Description of the form
alias|string|Used to generate the URL for the form
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the form should be published
publishDown|datetime/null|Date/time the form should be un published
dateAdded|datetime|Date/time form was created
createdBy|int|ID of the user that created the form
createdByUser|string|Name of the user that created the form
dateModified|datetime/null|Date/time form was last modified
modifiedBy|int|ID of the user that last modified the form
modifiedByUser|string|Name of the user that last modified the form
cachedHtml|string|Cached HTML for the form
template|string/null|Name of the template used to generate the HTML 
submissionCount|Number of times the form has been submitted
fields|array|Array of Field entities for the form. See below.
actions|array|Array of Action entities for the form. See below.

**Field Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the field
label|string|Label of the field
showLabel|bool|Display the label of the field
alias|string|Alias of the field (used as the database column)
type|string|Field type
defaultValue|string|Default value
isRequired|bool|Field is required
validationMessage|string|Validation message if required field is left empty
helpMessage|string|Help message for the field
order|int|Order of the field
properties|array|Configured properties for the field
labelAttributes|string/null|Custom HTML attributes for the label 
inputAttributes|Custom HTML attributes for the input
containerAttributes|Custom HTML attributes for the container

**Action Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the action
type|string|Action type
name|string|Name of the action
description|string/null|Description of the action
orde|int|Action order
propertie|array|Configured properties for the action

### List Forms
```php
<?php
// ...

$forms = $formApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir);
```
```json
{
    "total": 1,
    "forms": [
        {
            "id": 3,
            "name": "Newlsetter",
            "alias": "newsletter",
            "description": null,
            "isPublished": true,
            "publishUp": null,
            "publishDown": null,
            "dateAdded": "2015-07-15T15:06:02-05:00",
            "createdBy": 1,
            "createdByUser": "Joe Smith",
            "dateModified": "2015-07-20T13:11:56-05:00",
            "modifiedBy": 1,
            "modifiedByUser": "Joe Smith",
            "category": null,
            "cachedHtml": "\n\n<script...",
            "template": null,
            "submissionCount": 10,
            "fields": {
                "26": {
                    "id": 26,
                    "label": "Email",
                    "showLabel": false,
                    "alias": "email",
                    "type": "text",
                    "defaultValue": null,
                    "isRequired": true,
                    "validationMessage": "Email is required",
                    "helpMessage": null,
                    "order": 1,
                    "properties": {
                        "placeholder": "Email address"
                    },
                    "labelAttributes": null,
                    "inputAttributes": null,
                    "containerAttributes": null
                },
                "27": {
                    "id": 27,
                    "label": "Submit",
                    "showLabel": true,
                    "alias": "submit",
                    "type": "button",
                    "defaultValue": null,
                    "isRequired": false,
                    "validationMessage": null,
                    "helpMessage": null,
                    "order": 4,
                    "properties": [],
                    "labelAttributes": null,
                    "inputAttributes": null,
                    "containerAttributes": null
                }
            },
            "actions": {
                "4": {
                    "id": 4,
                    "type": "email.send.lead",
                    "name": "Send thank you email",
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

`GET /forms`

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

Same as [Get Form](#get-form).
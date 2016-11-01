## Campaigns
Use this endpoint to obtain details on Mautic's campaigns.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$auth     = ApiAuth::initiate($settings);
$apiUrl   = "https://your-mautic.com"; 
$campaignApi = MauticApi::getContext("campaigns", $auth, $apiUrl);
```

### Get Campaign
```php
<?php

//...
$campaign = $campaignApi->get($id);
```
```json
{
    "campaign": {
        "id": 3,
        "name": "Email A/B Test",
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
        "events": {
            "28": {
                "id": 28,
                "type": "lead.changepoints",
                "eventType": "action",
                "name": "Adjust lead points",
                "description": null,
                "order": 1,
                "properties": {
                  "points": 20
                },
                "triggerDate": null,
                "triggerInterval": 1,
                "triggerIntervalUnit": "d",
                "triggerMode": "immediate",
                "children": [],
                "parent": null,
                "decisionPath": null
            }
        }
    }
}
```
Get an individual campaign by ID.

#### HTTP Request

`GET /campaigns/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Campaign Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the campaign
name|string|Name of the campaign
description|string/null|Description of the campaign
alias|string|Used to generate the URL for the campaign
isPublished|bool|Published state
publishUp|datetime/null|Date/time when the campaign should be published
publishDown|datetime/null|Date/time the campaign should be un published
dateAdded|datetime|Date/time campaign was created
createdBy|int|ID of the user that created the campaign
createdByUser|string|Name of the user that created the campaign
dateModified|datetime/null|Date/time campaign was last modified
modifiedBy|int|ID of the user that last modified the campaign
modifiedByUser|string|Name of the user that last modified the campaign
events|array|Array of Event entities for the campaign. See below.

**Event Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the event
name|string|Name of the event
description|string|Optional description for the event
type|string|Type of event
eventType|string|"action" or "decision"
order|int|Order in relation to the other events (used for levels)
properties|object|Configured properties for the event
triggerMode|string|"immediate", "interval" or "date"
triggerDate|datetime/null|Date/time of when the event should trigger if triggerMode is "date"
triggerInterval|int/null|Interval for when the event should trigger 
triggerIntervalUnit|string|Interval unit for when the event should trigger. Options are i = minutes, h = hours, d = days, m = months, y = years
children|array|Array of this event's children ,
parent|object/null|This event's parent
decisionPath|string/null|If the event is connected into an action, this will be "no" for the non-decision path or "yes" for the actively followed path.

### List Campaigns
```php
<?php
// ...

$campaigns = $campaignApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir);
```
```json
{
    "total": 1,
    "campaigns": [
        {
            "id": 3,
            "name": "Welcome Campaign",
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
            "events": {
                "22": {
                    "id": 22,
                    "type": "email.send",
                    "eventType": "action",
                    "name": "Send welcome email",
                    "description": null,
                    "order": 1,
                    "properties": {
                        "email": 1
                    },
                    "triggerMode": "immediate",
                    "triggerDate": null,
                    "triggerInterval": null,
                    "triggerIntervalUnit": null,
                    "children": [],
                    "parent": null,
                    "decisionPath": null
                },
                "28": {
                    "id": 28,
                    "type": "lead.changepoints",
                    "eventType": "action",
                    "name": "Adjust lead points",
                    "description": null,
                    "order": 2,
                    "properties": {
                        "points": 20
                    },
                    "triggerMode": "immediate",                
                    "triggerDate": null,
                    "triggerInterval": null,
                    "triggerIntervalUnit": null,
                    "children": [],
                    "parent": null,
                    "decisionPath": null
                }
            }
        }
    ]
}
```
#### HTTP Request

`GET /campaigns`

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

Same as [Get Campaign](#get-campaign).


### Add Lead to a Campaign

```php
<?php

//...
$response = $campaignApi->addLead($leadId, $campaignId);
if (!isset($response['success'])) {
    // handle error
}
```
```json
{
    "success": true
}
```

Manually add a lead to a specific campaign.

#### HTTP Request

`POST /campaigns/CAMPAIGN_ID/lead/add/LEAD_ID`

#### Response

`Expected Response Code: 200`

See JSON code example.


### Remove Lead from a Campaign

```php
<?php

//...
$response = $listApi->removeLead($listId, $leadId);
if (!isset($response['success'])) {
    // handle error
}
```
```json
{
    "success": true
}
```

Manually remove a lead from a specific campaign.

#### HTTP Request

`POST /campaigns/CAMPAIGN_ID/lead/remove/LEAD_ID`

#### Response

`Expected Response Code: 200`

See JSON code example.
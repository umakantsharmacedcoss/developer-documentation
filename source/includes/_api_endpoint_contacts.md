## Contacts
Use this endpoint to manipulate and obtain details on Mautic's contacts.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$initAuth   = new ApiAuth();
$auth       = $initAuth->newAuth($settings);
$apiUrl     = "https://your-mautic.com";
$api        = new MauticApi();
$contactApi = $api->newApi("contacts", $auth, $apiUrl);
```

### Get Contact
```php
<?php

//...
$contact = $contactApi->get($id);
```
```json
    "contact": {
        "id": 47,
        "dateAdded": "2015-07-21T12:27:12-05:00",
        "createdBy": 1,
        "createdByUser": "Joe Smith",
        "dateModified": "2015-07-21T14:12:03-05:00",
        "modifiedBy": 1,
        "modifiedByUser": "Joe Smith",
        "owner": {
            "id": 1,
            "username": "joesmith",
            "firstName": "Joe",
            "lastName": "Smith"
        },
        "points": 10,
        "lastActive": "2015-07-21T14:19:37-05:00",
        "dateIdentified": "2015-07-21T12:27:12-05:00",
        "color": "ab5959",
        "ipAddresses": {
            "111.111.111.111": {
                "ipAddress": "111.111.111.111",
                "ipDetails": {
                    "city": "",
                    "region": "",
                    "country": "",
                    "latitude": "",
                    "longitude": "",
                    "isp": "",
                    "organization": "",
                    "timezone": ""
                }
            }
        },
        "fields": {
            "core": {
                "title": {
                    "id": "1",
                    "label": "Title",
                    "alias": "title",
                    "type": "lookup",
                    "group": "core",
                    "value": "Mr"
                },
                "firstname": {
                    "id": "2",
                    "label": "First Name",
                    "alias": "firstname",
                    "type": "text",
                    "group": "core",
                    "value": "Jim"
                },

                "...": {
                    "..." : "..."
                }

            },
            "social": {
                "twitter": {
                    "id": "17",
                    "label": "Twitter",
                    "alias": "twitter",
                    "type": "text",
                    "group": "social",
                    "value": "jimcontact"
                },

                "...": {
                    "..." : "..."
                }

            },
            "personal": [],
            "professional": [],
            "all": {
                "title": "Mr",
                "firstname": "Jim",
                "twitter": "jimcontact",

                "...": "..."
            }
        }
    }
```
Get an individual contact by ID.

#### HTTP Request

`GET /contacts/ID`

#### Response

`Expected Response Code: 200`

See JSON code example.

** Contact Properties **

Name|Type|Description
----|----|-----------
id|int|ID of the contact
dateAdded|datetime|Date/time contact was created
createdBy|int|ID of the user that created the contact
createdByUser|string|Name of the user that created the contact
dateModified|datetime/null|Date/time contact was last modified
modifiedBy|int|ID of the user that last modified the contact
modifiedByUser|string|Name of the user that last modified the contact
owner|object|User object that owns the contact.
points|int|Contact's current number of points
lastActive|datetime/null|Date/time for when the contact was last recorded as active
dateIdentified|datetime/null|Date/time when the contact identified themselves
color|string|Hex value given to contact from Point Trigger definitions based on the number of points the contact has been awarded
ipAddresses|array|Array of IPs currently associated with this contact
fields|array|Array of all contact fields with data grouped by field group. See JSON code example for format. This array includes an "all" key that includes an single level array of fieldAlias => contactValue pairs.

### List Contacts
```php
<?php

//...
$contacts = $contactApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);
```
```json
{
    "total": "1",
    "contacts": [
        {
            "id": 47,
            "isPublished": true,
            "dateAdded": "2015-07-21T12:27:12-05:00",
            "createdBy": 1,
            "createdByUser": "Joe Smith",
            "dateModified": "2015-07-21T14:12:03-05:00",
            "modifiedBy": 1,
            "modifiedByUser": "Joe Smith",
            "owner": {
                "id": 1,
                "username": "joesmith",
                "firstName": "Joe",
                "lastName": "Smith"
            },
            "points": 10,
            "lastActive": "2015-07-21T14:19:37-05:00",
            "dateIdentified": "2015-07-21T12:27:12-05:00",
            "color": "ab5959",
            "ipAddresses": {
                "111.111.111.111": {
                    "ipAddress": "111.111.111.111",
                    "ipDetails": {
                        "city": "",
                        "region": "",
                        "country": "",
                        "latitude": "",
                        "longitude": "",
                        "isp": "",
                        "organization": "",
                        "timezone": ""
                    }
                }
            },
            "fields": {
                "core": {
                    "title": {
                        "id": "1",
                        "label": "Title",
                        "alias": "title",
                        "type": "lookup",
                        "group": "core",
                        "value": "Mr"
                    },
                    "firstname": {
                        "id": "2",
                        "label": "First Name",
                        "alias": "firstname",
                        "type": "text",
                        "group": "core",
                        "value": "Jim"
                    },

                    "...": {
                        "..." : "..."
                    }
                },
                "social": {
                    "twitter": {
                        "id": "17",
                        "label": "Twitter",
                        "alias": "twitter",
                        "type": "text",
                        "group": "social",
                        "value": "jimcontact"
                    },

                    "...": {
                        "..." : "..."
                    }
                },
                "personal": [],
                "professional": [],
                "all": {
                    "title": "Mr",
                    "firstname": "Jim",
                    "twitter": "jimcontact",

                    "...": "..."
                }
            }
        }
    ]
}
```
Get a list of contacts.

#### HTTP Request

`GET /contacts`

** Query Parameters **

Name|Description
----|-----------
search|String or search command to filter entities by.
start|Starting row for the entities returned. Defaults to 0.
limit|Limit number of entities to return. Defaults to the system configuration for pagination (30).
orderBy|Column to sort by. Can use any column listed in the response.
orderByDir|Sort direction: asc or desc.
publishedOnly|Only return currently published entities.
minimal|Return only array of entities without additional lists in it.

#### Response

`Expected Response Code: 200`

See JSON code example.

** Properties **

Same as [Get Contact](#get-contact).

### Create Contact
```php
<?php

$data = array(
    'firstname' => 'Jim',
    'lastname'  => 'Contact',
    'email'     => 'jim@his-site.com',
    'ipAddress' => $_SERVER['REMOTE_ADDR']
);

$contact = $contactApi->create($data);
```
Create a new contact.

#### HTTP Request

`POST /contacts/new`

** Post Parameters **

Name|Description
----|-----------
*|Any contact field alias can be posted as a parameter.  For example, firstname, lastname, email, etc.
ipAddress|IP address to associate with the contact
lastActive|Date/time in UTC; preferablly in the format of Y-m-d H:m:i but if that format fails, the string will be sent through PHP's strtotime then formatted
owner|ID of a Mautic user to assign this contact to

#### Response

`Expected Response Code: 201`

** Properties **

Same as [Get Contact](#get-contact).

### Edit Contact
```php
<?php

$id   = 1;
$data = array(
    'email'     => 'jim-new-address@his-site.com',
    'ipAddress' => $_SERVER['REMOTE_ADDR']
);

// Create new a contact of ID 1 is not found?
$createIfNotFound = true;

$contact = $contactApi->edit($id, $data, $createIfNotFound);
```
Edit a new contact.  Note that this supports PUT or PATCH depending on the desired behavior.

** PUT ** creates a contact if the given ID does not exist and clears all the contact information, adds the information from the request.
**PATCH** fails if the contact with the given ID does not exist and updates the contact field values with the values form the request.

#### HTTP Request

To edit a contact and return a 404 if the contact is not found:

`PATCH /contacts/ID/edit`

To edit a contact and create a new one if the contact is not found:

`PUT /contacts/ID/edit`

** Post Parameters **

Name|Description
----|-----------
*|Any contact field alias can be posted as a parameter.  For example, firstname, lastname, email, etc.
ipAddress|IP address to associate with the contact
lastActive|Date/time in UTC; preferably in the format of Y-m-d H:m:i but if that format fails, the string will be sent through PHP's strtotime then formatted
owner|ID of a Mautic user to assign this contact to

#### Response

If `PUT`, the expected response code is `200` if the contact was edited or `201` if created.

If `PATCH`, the expected response code is `200`.

** Properties **

Same as [Get Contact](#get-contact).

### Delete Contact
```php
<?php

$contact = $contactApi->delete($id);
```
Delete a contact.

#### HTTP Request

`DELETE /contacts/ID/delete`

#### Response

`Expected Response Code: 200`

** Properties **

Same as [Get Contact](#get-contact).

### Add Do Not Contact
```php
<?php

$data = array(
     'eventName' => 'Score via api',
     'actionName' => 'Adding',
 );
$contactApi->addDnc($contactId, $channel, $reason, $channelId, $comments);
```

Add a contact to DNC list

#### HTTP Request

To add Do Not Contact entry to a contact:

`PATCH /contacts/ID/dnc/add/CHANNEL`

** Data Parameters (optional) **

Name|Description
----|-----------
channel|Channel of DNC. For example 'email', 'sms'... Default is email.
reason|Int value of the reason. Use Contacts constants: Contacts::UNSUBSCRIBED, Contacts::BOUNCED, Contacts::MANUAL. Default is Manual
channelId|ID of the entity which was the reason for unsubscription
comments|A text describing details of DNC entry

#### Response

Same as [Get Contact](#get-contact).

### Remove from Do Not Contact
```php
<?php
$contactApi->addDnc($contactId, $channel);
```

Remove a contact from DNC list

#### HTTP Request

To remove Do Not Contact entry from a contact:

`PATCH /contacts/ID/dnc/remove/CHANNEL`

** Data Parameters (optional) **

Name|Description
----|-----------
channel|Channel of DNC. For example 'email', 'sms'... Default is email.

#### Response

Same as [Get Contact](#get-contact).

### Add Points
```php
<?php

$data = array(
	 'eventName' => 'Score via api',
	 'actionName' => 'Adding',
 );
$contactApi->addPoints($contactId, $pointDelta, $data);
```

Add contact points

#### HTTP Request

To add points to a contact and return a 404 if the lead is not found:

`POST /contacts/ID/points/plus/POINTS`

** Data Parameters (optional) **

Name|Description
----|-----------
eventName|Name of the event
actionName|Name of the action

#### Response

`Expected Response Code: 200`
```json
{
    "success": true
}
```

### Subtract Points
```php
<?php

$data = array(
	 'eventname' => 'Score via api',
	 'actionname' => 'Subtracting',
 );
$contactApi->subtractPoints($contactId, $pointDelta, $data);
```
Subtract contact points

#### HTTP Request

To subtract points from a contact and return a 404 if the contact is not found:

`POST /contacts/ID/points/minus/POINTS`

** Data Parameters (optional) **

Name|Description
----|-----------
eventname|Name of the event
actionname|Name of the action

#### Response

`Expected Response Code: 200`
```json
{
    "success": true
}
```

### List Available Owners

```php
<?php

$owners = $contactApi->getOwners();
```
```json
[
  {
    "id": 1,
    "firstName": "Joe",
    "lastName": "Smith"
  },
  {
    "id": 2,
    "firstName": "Jane",
    "lastName": "Smith"
  }
]
```
Get a list of owners that can be used to assign contacts to when creating/editing.

#### HTTP Request

`GET /contacts/list/owners`

#### Response

`Expected Response Code: 200`

** Owner Properties **

Name|Type|Description
----|----|-----------
id|int|ID of the Mautic user
firstName|string|First name of the Mautic user
lastName|string|Last name of the Mautic user

### List Available Fields
```php
<?php

$fields = $contactApi->getFieldList();
```
```json
{
    "1": {
        "id": 1,
        "label": "Title",
        "alias": "title",
        "type": "lookup",
        "group": "core",
        "order": 1
    },
    "2": {
        "id": 2,
        "label": "First Name",
        "alias": "firstname",
        "type": "text",
        "group": "core",
        "order": 2
    },
    "3": {
        "id": 3,
        "label": "Last Name",
        "alias": "lastname",
        "type": "text",
        "group": "core",
        "order": 3
    },

    "...": {
        "..." : "..."
    }
}
```
Get a list of available contact fields including custom ones.

#### HTTP Request

`GET /contacts/list/fields`

#### Response

`Expected Response Code: 200`

** Field Properties **

Name|Type|Description
----|----|-----------
id|int|ID of the field
label|string|Field label
alias|string|Field alias used as the column name in the database
type|string|Type of field.  I.e. text, lookup, etc
group|string|Group the field belongs to
order|int|Field order

### List Contact Notes
```php
<?php

$notes = $contactApi->getContactNotes($id, $searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);
```
```json
{
    "total": 1,
    "notes": [
        {
              "id": 1,
              "text": "<p>Jim is super cool!</p>",
              "type": "general",
              "dateTime": "2015-07-23T13:14:00-05:00"
        }
    ]
}
```
Get a list of notes for a specific contact.

#### HTTP Request

`GET /contacts/ID/notes`

** Query Parameters **

Name|Description
----|-----------
search|String or search command to filter entities by.
start|Starting row for the entities returned. Defaults to 0.
limit|Limit number of entities to return. Defaults to the system configuration for pagination (30).
orderBy|Column to sort by. Can use any column listed in the response.
orderByDir|Sort direction: asc or desc.

#### Response

`Expected response code: 200`

** Note Properties **

Name|Type|Description
----|----|-----------
id|int|ID of the note
text|string|Body of the note
type|string|Type of note. Options are "general", "email", "call", "meeting"
dateTime|datetime|Date/time string of when the note was created.

### Get Segment Memberships
```php
<?php

$segments = $contactApi->getContactSegments($id);
```
```json
{
    "total": 1,
    "segments": {
        "3": {
            "id": 3,
            "name": "New Contacts",
            "alias": "newcontacts"
        }
    }
}
```
Get a list of contact segments the contact is a member of.

#### HTTP Request

`GET /contacts/ID/segments`

#### Response

`Expected response code: 200`

** List Properties **

Name|Type|Description
----|----|-----------
id|int|ID of the list
name|string|Name of the list
alias|string|Alias of the list used with search commands.
dateAdded|datetime|Date/time string for when the contact was added to the list
manuallyAdded|bool|True if the contact was manually added to the list versus being added by a filter
manuallyRemoved|bool|True if the contact was manually removed from the list even though the list's filter is a match

### Change List Memberships

See [Segements](#segments).


### Get Campaign Memberships
```php
<?php

$campaigns = $contactApi->getContactCampaigns($id);
```
```json
{
    "total": 1,
    "campaigns": {
        "1": {
            "id": 1,
            "name": "Welcome Campaign",
            "dateAdded": "2015-07-21T14:11:47-05:00",
            "manuallyRemoved": false,
            "manuallyAdded": false,
            "list_membership": [
                3
            ]
        }
    }
}
```
Get a list of campaigns the contact is a member of.

#### HTTP Request

`GET /contacts/ID/campaigns`

#### Response

`Expected response code: 200`

** List Properties **

Name|Type|Description
----|----|-----------
id|int|ID of the campaign
name|string|Name of the campaign
dateAdded|datetime|Date/time string for when the contact was added to the campaign
manuallyAdded|bool|True if the contact was manually added to the campaign versus being added by a contact list
manuallyRemoved|bool|True if the contact was manually removed from the campaign when the contact's list is assigned to the campaign
listMembership|array|Array of contact list IDs this contact belongs to that is also associated with this campaign

### Change Campaign Memberships

See [Campaigns](#campaigns).


### Get Contact's Events

This endpoint will get you all events for a specific contact.

```php
<?php

$events = $contactApi->getEvents($id, $search, $includeEvents, $excludeEvents, $orderBy, $orderByDir, $page);
```
** Query Parameters **

Name|Description
----|-----------
id|Contact ID
filters[search]|String or search command to filter events by.
filters[includeEvents][]|Array of event types to include.
filters[excludeEvents][]|Array of event types to exclude.
orderBy|Column to sort by. Can use any column listed in the response.
orderByDir|Sort direction: asc or desc.
page|What page number to load

```json
{
  "events":[
    {
      "event":"lead.identified",
      "icon":"fa-user",
      "eventType":"Contact identified",
      "eventPriority":-4,
      "timestamp":"2016-06-09T21:39:08+00:00",
      "featured":true
    }
  ],
  "filters":{
    "search":"",
    "includeEvents":[
      "lead.identified"
    ],
    "excludeEvents":[]
  },
  "order":[
    "",
    "ASC"
  ],
  "types":{
    "lead.ipadded":"Accessed from IP",
    "asset.download":"Asset downloaded",
    "campaign.event":"Campaign action triggered",
    "lead.create":"Contact created",
    "lead.identified":"Contact identified",
    "lead.donotcontact":"Do not contact",
    "email.read":"Email read",
    "email.sent":"Email sent",
    "email.failed":"Failed",
    "form.submitted":"Form submitted",
    "page.hit":"Page hit",
    "point.gained":"Point gained",
    "stage.changed":"Stage changed",
    "lead.utmtagsadded":"UTM tags recorded",
    "page.videohit":"Video View Event"
  },
  "total":1,
  "page":1,
  "limit":25,
  "maxPages":1
}
```
Get a list of contact events the contact created.

#### HTTP Request

`GET /contacts/ID/events`

#### Response

`Expected response code: 200`

** List Properties **

Name|Type|Description
----|----|-----------
events|array|List of events
event|string|ID of the event type
icon|string|Icon class from FontAwesome
eventType|string|Human name of the event
eventPriority|string|Priority of the event
timestamp|timestamp|Date and time when the event was created
featured|bool|Flag whether the event is featured
filters|array|Filters used in the query
order|array|Ordering used in the query
types|array|Array of available event types
total|int|Total number of events in the request
page|int|Current page number
limit|int|Limit of events per page
maxPages|int|How many pages of events are there

### Get All Contact Events

This endpoint will get you all events across all contacts.

```php
<?php

$events = $contactApi->getAllEvents($search, $includeEvents, $excludeEvents, $orderBy, $orderByDir, $page);
```
** Query Parameters **

Name|Description
----|-----------
filters[search]|String or search command to filter events by.
filters[includeEvents][]|Array of event types to include.
filters[excludeEvents][]|Array of event types to exclude.
orderBy|Column to sort by. Can use any column listed in the response.
orderByDir|Sort direction: asc or desc.
page|What page number to load

```json
{
  "events": [
    {
      "event": "stage.changed",
      "eventLabel": {
        "label": "12:test",
        "href": "\/s\/stages\/edit\/12",
        "isExternal": false
      },
      "eventType": "Stage changed",
      "timestamp": "2017-08-03T10:27:50+00:00",
      "extra": {
        "log": {
          "reference": "12",
          "eventName": "12:test",
          "actionName": "Stage changed",
          "dateAdded": "2017-08-03T10:27:50+00:00",
          "lead_id": "7149"
        }
      },
      "icon": "fa-tachometer",
      "contactId": "7149"
    },
    {
      "event": "lead.utmtagsadded",
      "eventType": "UTM tags recorded",
      "timestamp": "2017-07-31T20:20:20+00:00",
      "icon": "fa-tag",
      "extra": {
        "utmtags": {
          "id": "1",
          "lead_id": "7151",
          "date_added": "2017-07-31T20:20:20+00:00",
          "query": [],
          "referer": "http:\/\/seznam.cz"
        }
      },
      "contentTemplate": "MauticLeadBundle:SubscribedEvents\\Timeline:utmadded.html.php",
      "contactId": "7151"
    },
    {
      "event": "point.gained",
      "eventLabel": "test \/ 3",
      "eventType": "Point gained",
      "timestamp": "2017-07-29T20:20:20+00:00",
      "extra": {
        "log": {
          "eventName": "test",
          "actionName": "test",
          "dateAdded": "2017-07-29T20:20:20+00:00",
          "type": "test",
          "delta": "3",
          "id": "1",
          "lead_id": "7151"
        }
      },
      "icon": "fa-calculator",
      "contactId": "7151"
    },
    {
      "event": "campaign.event",
      "eventLabel": {
        "label": "Send Email To User \/ test user send email action",
        "href": "\/s\/campaigns\/view\/30"
      },
      "eventType": "Campaign action triggered",
      "timestamp": "2017-07-27T14:21:10+00:00",
      "extra": {
        "log": {
          "event_id": "98",
          "campaign_id": "30",
          "dateTriggered": "2017-07-27T14:21:10+00:00",
          "event_name": "Send Email To User",
          "campaign_name": "test user send email action",
          "campaign_description": "Created via API",
          "metadata": [],
          "type": "email.send.to.user",
          "isScheduled": "0",
          "lead_id": "1190"
        },
        "campaignEventSettings": {
          "decision": {
            "asset.download": {
              "label": "Downloads asset",
              "description": "Trigger actions upon downloading an asset.",
              "eventName": "mautic.asset.on_campaign_trigger_decision",
              "formType": "campaignevent_assetdownload",
              "channel": "asset",
              "channelIdField": "assets"
            },
            "email.open": {
              "label": "Opens email",
              "description": "Trigger actions when an email is opened. Connect a &quot;Send Email&quot; action to the top of this decision.",
              "eventName": "mautic.email.on_campaign_trigger_decision",
              "connectionRestrictions": {
                "source": {
                  "action": [
                    "email.send"
                  ]
                }
              }
            },
            "dwc.decision": {
              "label": "Request dynamic content",
              "description": "This is the top level for a dynamic content request.",
              "eventName": "mautic.dwc.on_campaign_trigger_decision",
              "formType": "dwcdecision_list",
              "formTypeOptions": {
                "update_select": "campaignevent_properties_dynamicContent"
              },
              "formTheme": "MauticDynamicContentBundle:FormTheme\\DynamicContentDecisionList",
              "channel": "dynamicContent",
              "channelIdField": "dynamicContent"
            },
            "form.submit": {
              "label": "Submits form",
              "description": "Trigger actions when a contact submits a form",
              "formType": "campaignevent_formsubmit",
              "eventName": "mautic.form.on_campaign_trigger_decision"
            },
            "page.pagehit": {
              "label": "Visits a page",
              "description": "Trigger actions on a page\/url hit.",
              "formType": "campaignevent_pagehit",
              "eventName": "mautic.page.on_campaign_trigger_decision",
              "channel": "page",
              "channelIdField": "pages"
            }
          },
          "condition": {
            "lead.field_value": {
              "label": "Contact field value",
              "description": "Condition based on a contact field value.",
              "formType": "campaignevent_lead_field_value",
              "formTheme": "MauticLeadBundle:FormTheme\\FieldValueCondition",
              "eventName": "mautic.lead.on_campaign_trigger_condition"
            },
            "lead.owner": {
              "label": "Contact owner",
              "description": "Condition based on a contact owner.",
              "formType": "campaignevent_lead_owner",
              "eventName": "mautic.lead.on_campaign_trigger_condition"
            },
            "lead.segments": {
              "label": "Contact segments",
              "description": "Condition based on a contact segments.",
              "formType": "campaignevent_lead_segments",
              "eventName": "mautic.lead.on_campaign_trigger_condition"
            },
            "lead.tags": {
              "label": "Contact tags",
              "description": "Condition based on a contact tags.",
              "formType": "campaignevent_lead_tags",
              "eventName": "mautic.lead.on_campaign_trigger_condition"
            },
            "form.field_value": {
              "label": "Form field value",
              "description": "Trigger actions when a submitted form field value suits the defined condition.",
              "formType": "campaignevent_form_field_value",
              "formTheme": "MauticFormBundle:FormTheme\\FieldValueCondition",
              "eventName": "mautic.form.on_campaign_trigger_condition"
            }
          },
          "action": {
            "lead.scorecontactscompanies": {
              "label": "Add to company's score",
              "description": "This action will add the specified value to the company's existing score",
              "formType": "scorecontactscompanies_action",
              "eventName": "mautic.lead.on_campaign_trigger_action"
            },
            "lead.addtocompany": {
              "label": "Add to company action",
              "description": "This action will add contacts to the selected company",
              "formType": "addtocompany_action",
              "eventName": "mautic.lead.on_campaign_trigger_action"
            },
            "lead.changepoints": {
              "label": "Adjust contact points",
              "description": "Increase or decrease points",
              "formType": "leadpoints_action",
              "eventName": "mautic.lead.on_campaign_trigger_action"
            },
            "campaign.addremovelead": {
              "label": "Change campaigns",
              "description": "Add contact to specific campaigns and\/or remove from specific campaigns when the event is triggered.",
              "formType": "campaignevent_addremovelead",
              "formTypeOptions": {
                "include_this": true
              },
              "callback": "\\Mautic\\CampaignBundle\\Helper\\CampaignEventHelper::addRemoveLead"
            },
            "stage.change": {
              "label": "Change contact's stage",
              "description": "Choose a stage to change a contact to.",
              "eventName": "mautic.stage.on_campaign_trigger_action",
              "formType": "stageaction_change",
              "formTheme": "MauticStageBundle:FormTheme\\StageActionChange"
            },
            "lead.deletecontact": {
              "label": "Delete contact",
              "description": "<span class='text-danger'>Permanently deletes the contact as well as all associated statistical data. <strong>Warning: this is irreversible!<\/strong><\/span>",
              "eventName": "mautic.lead.on_campaign_trigger_action",
              "connectionRestrictions": {
                "target": {
                  "decision": [
                    "none"
                  ],
                  "action": [
                    "none"
                  ],
                  "condition": [
                    "none"
                  ]
                }
              }
            },
            "lead.changelist": {
              "label": "Modify contact's segments",
              "description": "Add contact to or remove contact from segment(s)",
              "formType": "leadlist_action",
              "eventName": "mautic.lead.on_campaign_trigger_action"
            },
            "lead.changetags": {
              "label": "Modify contact's tags",
              "description": "Add tag to or remove tag from contact",
              "formType": "modify_lead_tags",
              "eventName": "mautic.lead.on_campaign_trigger_action"
            },
            "plugin.leadpush": {
              "label": "Push contact to integration",
              "description": "Push a contact to the selected integration.",
              "formType": "integration_list",
              "formTheme": "MauticPluginBundle:FormTheme\\Integration",
              "eventName": "mautic.plugin.on_campaign_trigger_action"
            },
            "dwc.push_content": {
              "label": "Push dynamic content",
              "description": "Trigger actions when a Dynamic Content slot is requested.",
              "eventName": "mautic.dwc.on_campaign_trigger_action",
              "formType": "dwcsend_list",
              "formTypeOptions": {
                "update_select": "campaignevent_properties_dynamicContent"
              },
              "formTheme": "MauticDynamicContentBundle:FormTheme\\DynamicContentPushList",
              "timelineTemplate": "MauticDynamicContentBundle:SubscribedEvents\\Timeline:index.html.php",
              "hideTriggerMode": true,
              "connectionRestrictions": {
                "anchor": [
                  "decision.inaction"
                ],
                "source": {
                  "decision": [
                    "dwc.decision"
                  ]
                }
              },
              "channel": "dynamicContent",
              "channelIdField": "dwc_slot_name"
            },
            "email.send": {
              "label": "Send email",
              "description": "Send the selected email to the contact.",
              "eventName": "mautic.email.on_campaign_trigger_action",
              "formType": "emailsend_list",
              "formTypeOptions": {
                "update_select": "campaignevent_properties_email",
                "with_email_types": true
              },
              "formTheme": "MauticEmailBundle:FormTheme\\EmailSendList",
              "channel": "email",
              "channelIdField": "email"
            },
            "message.send": {
              "label": "Send marketing message",
              "description": "Send a message through the configured channels withing the marketing message selected.",
              "eventName": "mautic.channel.on_campaign_trigger_action",
              "formType": "message_send",
              "formTheme": "MauticChannelBundle:FormTheme\\MessageSend",
              "channel": "channel.message",
              "channelIdField": "marketingMessage",
              "connectionRestrictions": {
                "target": {
                  "decision": [
                    "email.open",
                    "page.pagehit",
                    "asset.download",
                    "form.submit"
                  ]
                }
              },
              "timelineTemplate": "MauticChannelBundle:SubscribedEvents\\Timeline:index.html.php",
              "timelineTemplateVars": {
                "messageSettings": {
                  "email": {
                    "campaignAction": "email.send",
                    "campaignDecisionsSupported": [
                      "email.open",
                      "page.pagehit",
                      "asset.download",
                      "form.submit"
                    ],
                    "lookupFormType": "email_list",
                    "label": "Email"
                  },
                  "sms": {
                    "campaignAction": "sms.send_text_sms",
                    "campaignDecisionsSupported": [
                      "page.pagehit",
                      "asset.download",
                      "form.submit"
                    ],
                    "lookupFormType": "sms_list",
                    "repository": "MauticSmsBundle:Sms",
                    "label": "Text Message"
                  },
                  "mobile_notification": {
                    "campaignAction": "notification.send_mobile_notification",
                    "campaignDecisionsSupported": [
                      "page.pagehit",
                      "asset.download",
                      "form.submit"
                    ],
                    "lookupFormType": "notification_list",
                    "repository": "MauticNotificationBundle:Notification",
                    "lookupOptions": {
                      "mobile": true,
                      "desktop": false
                    },
                    "label": "Mobile_notification"
                  }
                }
              }
            },
            "sms.send_text_sms": {
              "label": "Send text message",
              "description": "Sends a text\/sms to the contact.",
              "eventName": "mautic.sms.on_campaign_trigger_action",
              "formType": "smssend_list",
              "formTypeOptions": {
                "update_select": "campaignevent_properties_sms"
              },
              "formTheme": "MauticSmsBundle:FormTheme\\SmsSendList",
              "timelineTemplate": "MauticSmsBundle:SubscribedEvents\\Timeline:index.html.php",
              "channel": "sms",
              "channelIdField": "sms"
            },
            "twitter.tweet": {
              "label": "Tweet contact",
              "description": "Send tweets automatically to contacts. This must be in compliance with  <a href='https:\/\/support.twitter.com\/articles\/76915#Tweets' target='_blank'>Twitter's Automation Rules<\/a>.",
              "eventName": "mautic.social.on_campaign_trigger_action",
              "formTypeOptions": {
                "update_select": "campaignevent_properties_channelId"
              },
              "formType": "tweetsend_list",
              "channel": "social.tweet",
              "channelIdField": "channelId"
            },
            "lead.updatelead": {
              "label": "Update contact",
              "description": "Update the current contact's fields with the defined values from this action",
              "formType": "updatelead_action",
              "formTheme": "MauticLeadBundle:FormTheme\\ActionUpdateLead",
              "eventName": "mautic.lead.on_campaign_trigger_action"
            }
          },
          "connectionRestrictions": {
            "anchor": {
              "decision": {
                "dwc.push_content": [
                  "inaction"
                ]
              }
            },
            "asset.download": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "email.open": {
              "source": {
                "decision": [],
                "condition": [],
                "action": [
                  "email.send"
                ]
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "dwc.decision": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "form.submit": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "page.pagehit": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.field_value": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.owner": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.segments": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.tags": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "form.field_value": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.scorecontactscompanies": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.addtocompany": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.changepoints": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "campaign.addremovelead": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "stage.change": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.deletecontact": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [
                  "none"
                ],
                "condition": [
                  "none"
                ],
                "action": [
                  "none"
                ]
              }
            },
            "lead.changelist": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.changetags": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "plugin.leadpush": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "dwc.push_content": {
              "source": {
                "decision": [
                  "dwc.decision"
                ],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "email.send": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "message.send": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [
                  "email.open",
                  "page.pagehit",
                  "asset.download",
                  "form.submit"
                ],
                "condition": [],
                "action": []
              }
            },
            "sms.send_text_sms": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "twitter.tweet": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            },
            "lead.updatelead": {
              "source": {
                "decision": [],
                "condition": [],
                "action": []
              },
              "target": {
                "decision": [],
                "condition": [],
                "action": []
              }
            }
          }
        }
      },
      "contentTemplate": "MauticCampaignBundle:SubscribedEvents\\Timeline:index.html.php",
      "icon": "fa-clock-o",
      "contactId": "1190"
    },
    {
      "event": "asset.download",
      "eventLabel": {
        "label": "test",
        "href": "\/s\/assets\/view\/1"
      },
      "extra": {
        "asset": {
          "isPublished": true,
          "dateAdded": "2017-07-27T10:05:16+00:00",
          "dateModified": "2017-08-01T14:12:45+00:00",
          "createdBy": 1,
          "createdByUser": "John Doe",
          "modifiedBy": 1,
          "modifiedByUser": "John Doe",
          "id": 1,
          "title": "test",
          "alias": "test",
          "category": {
            "isPublished": true,
            "dateAdded": "2017-08-01T14:12:42+00:00",
            "createdBy": 1,
            "createdByUser": "John Doe",
            "id": 29,
            "title": "test",
            "alias": "test",
            "bundle": "asset"
          },
          "language": "en",
          "downloadCount": 4,
          "uniqueDownloadCount": 2,
          "revision": 3,
          "extension": "png",
          "mime": "image\/png",
          "size": 38925,
          "storageLocation": "local"
        },
        "assetDownloadUrl": "http:\/\/mautic.dev\/asset\/1:test"
      },
      "eventType": "Asset downloaded",
      "timestamp": "2017-07-27T10:05:46+00:00",
      "icon": "fa-download",
      "contentTemplate": "MauticAssetBundle:SubscribedEvents\\Timeline:index.html.php",
      "contactId": "7152"
    },
    {
      "event": "lead.imported",
      "eventType": "Imported",
      "eventLabel": {
        "label": "Contact imported from Mautic_July15.csv",
        "href": "\/s\/contacts\/import\/view\/5"
      },
      "timestamp": "2017-07-20T16:12:21+00:00",
      "icon": "fa-download",
      "extra": {
        "id": "7895",
        "lead_id": "7083",
        "user_id": "1",
        "user_name": "John Doe",
        "bundle": "lead",
        "object": "import",
        "action": "inserted",
        "object_id": "5",
        "date_added": "2017-07-20T16:12:21+00:00",
        "properties": {
          "line": 4900,
          "file": "Mautic_July15.csv"
        }
      },
      "contentTemplate": "MauticLeadBundle:SubscribedEvents\\Timeline:import.html.php",
      "contactId": "7083"
    },
    {
      "event": "form.submitted",
      "eventLabel": {
        "label": "test",
        "href": "\/s\/forms\/view\/25"
      },
      "eventType": "Form submitted",
      "timestamp": "2017-07-17T09:52:29+00:00",
      "extra": {
        "submission": {
          "id": 1,
          "ipAddress": {
            "ip": "127.0.0.1",
            "id": 1,
            "ipDetails": {
              "city": "",
              "region": "",
              "zipcode": "",
              "country": "",
              "latitude": "",
              "longitude": "",
              "isp": "",
              "organization": "",
              "timezone": "",
              "extra": ""
            }
          },
          "form": {
            "isPublished": true,
            "dateAdded": "2017-07-17T07:42:35+00:00",
            "dateModified": "2017-07-28T08:12:22+00:00",
            "createdBy": 1,
            "createdByUser": "John Doe",
            "modifiedBy": 1,
            "modifiedByUser": "John Doe",
            "id": 25,
            "name": "test",
            "alias": "test",
            "cachedHtml": "\n\n<style type=\"text\/css\" scoped>\n ...",
            "fields": [
              {
                "id": 25,
                "label": "email",
                "showLabel": true,
                "alias": "email",
                "type": "email",
                "isRequired": false,
                "order": 1,
                "properties": [],
                "leadField": "email",
                "saveResult": true,
                "isAutoFill": true
              },
              {
                "id": 26,
                "label": "Submit",
                "showLabel": true,
                "alias": "submit",
                "type": "button",
                "isRequired": false,
                "order": 2,
                "properties": [],
                "inputAttributes": "class=\"btn btn-default\"",
                "saveResult": true,
                "isAutoFill": false
              }
            ],
            "actions": [
              {
                "id": 25,
                "name": "Send form results",
                "type": "form.email",
                "order": 1,
                "properties": {
                  "subject": "Your form results",
                  "copy_lead": 1,
                  "message": "<strong>email<\/strong>: ...",
                  "templates": "",
                  "to": "test@test.test"
                }
              }
            ],
            "inKioskMode": true,
            "renderStyle": true,
            "formType": "standalone",
            "postAction": "return"
          },
          "lead": {
            "isPublished": true,
            "dateAdded": "2017-07-17T09:52:29+00:00",
            "dateModified": "2017-07-27T09:02:34+00:00",
            "createdBy": 1,
            "createdByUser": "John Doe",
            "modifiedBy": 1,
            "modifiedByUser": "John Doe",
            "id": 2183,
            "points": 0,
            "email": "test@test.test",
            "fields": [],
            "lastActive": "2017-07-17T09:52:29+00:00",
            "ipAddresses": [
              {
                "ip": "127.0.0.1",
                "id": 1,
                "ipDetails": {
                  "city": "",
                  "region": "",
                  "zipcode": "",
                  "country": "",
                  "latitude": "",
                  "longitude": "",
                  "isp": "",
                  "organization": "",
                  "timezone": "",
                  "extra": ""
                }
              }
            ],
            "tags": [],
            "utmtags": [],
            "dateIdentified": "2017-07-17T09:52:29+00:00",
            "doNotContact": [
              {
                "id": 1,
                "dateAdded": "2017-07-27T09:02:34+00:00",
                "reason": 2,
                "comments": "Soft bounce",
                "channel": "email"
              }
            ],
            "frequencyRules": [
              {
                "channel": "email",
                "preferredChannel": true,
                "dateAdded": "2017-07-25T14:58:57+00:00"
              },
              {
                "channel": "sms",
                "preferredChannel": false,
                "dateAdded": "2017-07-25T14:58:57+00:00"
              }
            ]
          },
          "dateSubmitted": "2017-07-17T09:52:29+00:00",
          "referer": "http:\/\/mautic.dev\/s\/forms\/preview\/25",
          "results": {
            "form_id": "25",
            "email": "test@test.test"
          }
        },
        "form": {
          "isPublished": true,
          "dateAdded": "2017-07-17T07:42:35+00:00",
          "dateModified": "2017-07-28T08:12:22+00:00",
          "createdBy": 1,
          "createdByUser": "John Doe",
          "modifiedBy": 1,
          "modifiedByUser": "John Doe",
          "id": 25,
          "name": "test",
          "alias": "test",
          "cachedHtml": "\n\n<style type=\"text\/css\" scoped>\n ...",
          "fields": [
            {
              "id": 25,
              "label": "email",
              "showLabel": true,
              "alias": "email",
              "type": "email",
              "isRequired": false,
              "order": 1,
              "properties": [],
              "leadField": "email",
              "saveResult": true,
              "isAutoFill": true
            },
            {
              "id": 26,
              "label": "Submit",
              "showLabel": true,
              "alias": "submit",
              "type": "button",
              "isRequired": false,
              "order": 2,
              "properties": [],
              "inputAttributes": "class=\"btn btn-default\"",
              "saveResult": true,
              "isAutoFill": false
            }
          ],
          "actions": [
            {
              "id": 25,
              "name": "Send form results",
              "type": "form.email",
              "order": 1,
              "properties": {
                "subject": "Your form results",
                "copy_lead": 1,
                "message": "<strong>email<\/strong>: ...",
                "templates": "",
                "to": "test@test.test"
              }
            }
          ],
          "inKioskMode": true,
          "renderStyle": true,
          "formType": "standalone",
          "postAction": "return"
        },
        "page": {
          "isPublished": true,
          "language": "en",
          "hits": 0,
          "uniqueHits": 0,
          "variantHits": 0,
          "revision": 1,
          "variantSettings": [],
          "variantChildren": [],
          "translationChildren": []
        }
      },
      "contentTemplate": "MauticFormBundle:SubscribedEvents\\Timeline:index.html.php",
      "icon": "fa-pencil-square-o",
      "contactId": "2183"
    },
    {
      "event": "email.sent",
      "eventLabel": {
        "label": "Custom Email: API test email - SendToSegment test",
        "href": "\/email\/view\/5966744653272",
        "isExternal": true
      },
      "eventType": "Email sent",
      "timestamp": "2017-07-12T19:11:02+00:00",
      "dateSent": "2017-07-12T19:11:02+00:00",
      "extra": {
        "stat": {
          "id": "5",
          "dateSent": "2017-07-12T19:11:02+00:00",
          "isRead": "0",
          "isFailed": "0",
          "viewedInBrowser": "0",
          "retryCount": "0",
          "idHash": "5966744653272",
          "openDetails": [],
          "storedSubject": "API test email - SendToSegment test",
          "timeToRead": false
        },
        "type": "sent"
      },
      "contentTemplate": "MauticEmailBundle:SubscribedEvents\\Timeline:index.html.php",
      "icon": "fa-envelope"
    },
    {
      "event": "page.hit",
      "eventLabel": {
        "label": "test",
        "href": "\/s\/pages\/view\/1"
      },
      "eventType": "Page hit",
      "timestamp": "2017-07-12T15:38:07+00:00",
      "extra": {
        "hit": {
          "page_id": "1",
          "userAgent": "Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/59.0.3071.115 Safari\/537.36",
          "dateHit": "2017-07-12T15:38:07+00:00",
          "url": "http:\/\/mautic.dev\/test",
          "query": {
            "page_url": "http:\/\/mautic.dev\/test"
          },
          "clientInfo": "a:6:{s:4:\"type\";s:7:\"browser\";s:4:\"name\";s:6:\"Chrome\";s:10:\"short_name\";s:2:\"CH\";s:7:\"version\";s:4:\"59.0\";s:6:\"engine\";s:5:\"Blink\";s:14:\"engine_version\";s:0:\"\";}",
          "device": "desktop",
          "deviceOsName": "Mac",
          "deviceBrand": "",
          "deviceModel": ""
        }
      },
      "contentTemplate": "MauticPageBundle:SubscribedEvents\\Timeline:index.html.php",
      "icon": "fa-link",
      "contactId": "2"
    }
  ],
  "filters": {
    "search": "",
    "includeEvents": [],
    "excludeEvents": []
  },
  "order": [
    "timestamp",
    "DESC"
  ],
  "types": {
    "lead.ipadded": "Accessed from IP",
    "asset.download": "Asset downloaded",
    "campaign.event": "Campaign action triggered",
    "campaign.event.scheduled": "Campaign event scheduled",
    "lead.create": "Contact created",
    "lead.identified": "Contact identified",
    "lead.donotcontact": "Do not contact",
    "email.failed": "Email failed",
    "email.read": "Email read",
    "email.sent": "Email sent",
    "form.submitted": "Form submitted",
    "lead.imported": "Imported",
    "page.hit": "Page hit",
    "point.gained": "Point gained",
    "stage.changed": "Stage changed",
    "lead.utmtagsadded": "UTM tags recorded",
    "page.videohit": "Video view event"
  },
  "total": 5967,
  "page": 1,
  "limit": 25,
  "maxPages": 237
}
```

#### HTTP Request

`GET /contacts/events`

#### Response

`Expected response code: 200`

** List Properties **

Name|Type|Description
----|----|-----------
events|array|List of events
event|string|ID of the event type
icon|string|Icon class from FontAwesome
eventType|string|Human name of the event
eventPriority|string|Priority of the event
contactId|ID of the contact who created the event
timestamp|timestamp|Date and time when the event was created
featured|bool|Flag whether the event is featured
filters|array|Filters used in the query
order|array|Ordering used in the query
types|array|Array of available event types
total|int|Total number of events in the request
page|int|Current page number
limit|int|Limit of events per page
maxPages|int|How many pages of events are there

### Get Contact's Companies
```php
<?php

$companies = $contactApi->getContactCompanies($contactId);

```json
{
  "total":1,
  "companies":[
    {
      "company_id":"420",
      "date_associated":"2016-12-27 15:03:43",
      "is_primary":"0",
      "companyname":"test",
      "companyemail":"test@company.com",
      "companycity":"Raleigh",
      "score":"0",
      "date_added":"2016-12-27 15:03:42"
    }
  ]
}
```
Get a list of contact's companies the contact belongs to.

#### HTTP Request

`GET /contacts/ID/companies`

#### Response

`Expected response code: 200`

**List Properties**

Name|Type|Description
----|----|-----------
company_id|int|Company ID
date_associated|datetime|Date and time when the contact was associated to the company
date_added|datetime|Date and time when the company was created
is_primary|bool|Flag whether the company association is primary (current)
companyname|string|Name of the company
companyemail|string|Email of the company
companycity|string|City of the company
score|int|Score of the company


### Get Contact's Devices
```php
<?php

$devices = $contactApi->getContactDevices($contactId);

```json
{
  "total":1,
  "devices":[
    {
      "id":60,
      "lead":[],
      "clientInfo":[],
      "device":"desktop",
      "deviceOsName":"Ubuntu",
      "deviceOsShortName":"UBT",
      "deviceOsPlatform":"x64"
    }
  ]
}
```
Get a list of contact's devices the contact has used.

#### HTTP Request

`GET /contacts/ID/devices`

#### Response

`Expected response code: 200`

**List Properties**

Name|Type|Description
----|----|-----------
id|int|Device ID
clientInfo|array|Array with various information about the client (browser)
device|string|Device type; desktop, mobile..
deviceOsName|string|Full device OS name
deviceOsShortName|string|Short device OS name
deviceOsPlatform|string|OS platform

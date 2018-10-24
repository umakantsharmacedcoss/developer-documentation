## Reports
Use this endpoint to obtain details on Mautic's reports.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$initAuth  = new ApiAuth();
$auth      = $initAuth->newAuth($settings);
$apiUrl    = "https://your-mautic.com";
$api       = new MauticApi();
$reportApi = $api->newApi("reports", $auth, $apiUrl);
```

### Get Report
```php
<?php

//...
// Simple get all with default options:
$report = $reportApi->get($id);

// Or define exactly what rows you want:
$limit    = 100;
$page     = 2;
$dateFrom = \DateTime('1 week ago');
$dateTo   = \DateTime('now');
$report   = $reportApi->get($id, $limit, $page, $dateFrom, $dateTo);
```
```json
{
  "totalResults": 3990,
  "data": [
    {
      "id2": "12",
      "email1": "mautic-9@gregy.cz",
      "firstname1": "",
      "lastname1": ""
    },
    {
      "id2": "23",
      "email1": "mautic-20@gregy.cz",
      "firstname1": "",
      "lastname1": ""
    },
    {
      "id2": "24",
      "email1": "mautic-21@gregy.cz",
      "firstname1": "",
      "lastname1": ""
    },
    {
      "id2": "25",
      "email1": "mautic-22@gregy.cz",
      "firstname1": "",
      "lastname1": ""
    },
    {
      "id2": "26",
      "email1": "mautic-23@gregy.cz",
      "firstname1": "",
      "lastname1": ""
    }
  ],
  "dataColumns": {
    "address11": "l.address1",
    "address21": "l.address2",
    "attribution1": "l.attribution",
    "attribution_date1": "l.attribution_date",
    "birthday1": "l.birthday",
    "boolean1": "l.boolean",
    "city1": "l.city",
    "company1": "l.company",
    "additional_information1": "comp.additional_information",
    "companyaddress11": "comp.companyaddress1",
    "companyaddress21": "comp.companyaddress2",
    "companyannual_revenue1": "comp.companyannual_revenue",
    "business_type1": "comp.business_type",
    "companycity1": "comp.companycity",
    "companyemail1": "comp.companyemail",
    "companyname1": "comp.companyname",
    "companycountry1": "comp.companycountry",
    "companydescription1": "comp.companydescription",
    "companyfax1": "comp.companyfax",
    "how_did_you_hear_about_us1": "comp.how_did_you_hear_about_us",
    "id1": "comp.id",
    "companyindustry1": "comp.companyindustry",
    "no_of_users1": "comp.no_of_users",
    "companynumber_of_employees1": "comp.companynumber_of_employees",
    "number_of_countries1": "comp.number_of_countries",
    "number_of_locationsschool1": "comp.number_of_locationsschool",
    "number_of_students_per_ye1": "comp.number_of_students_per_ye",
    "companyphone1": "comp.companyphone",
    "companystate1": "comp.companystate",
    "companywebsite1": "comp.companywebsite",
    "why_new_software_needed1": "comp.why_new_software_needed",
    "companyzipcode1": "comp.companyzipcode",
    "company_multi1": "comp.company_multi",
    "system_currently_used1": "comp.system_currently_used",
    "system_required_by1": "comp.system_required_by",
    "id2": "l.id",
    "country1": "l.country",
    "custom_select1": "l.custom_select",
    "date_identified1": "l.date_identified",
    "date_test1": "l.date_test",
    "email1": "l.email",
    "facebook1": "l.facebook",
    "fax1": "l.fax",
    "firstname1": "l.firstname",
    "foursquare1": "l.foursquare",
    "gender1": "l.gender",
    "googleplus1": "l.googleplus",
    "ip_address1": "i.ip_address",
    "instagram1": "l.instagram",
    "is_primary1": "companies_lead.is_primary",
    "lastname1": "l.lastname",
    "linkedin1": "l.linkedin",
    "mobile1": "l.mobile",
    "multiline1": "l.multiline",
    "multiselect1": "l.multiselect",
    "owner_id1": "l.owner_id",
    "first_name1": "u.first_name",
    "last_name1": "u.last_name",
    "phone1": "l.phone",
    "points1": "l.points",
    "position1": "l.position",
    "preferred_locale1": "l.preferred_locale",
    "timezone1": "l.timezone",
    "shipping_textarea1": "l.shipping_textarea",
    "shipping_bool1": "l.shipping_bool",
    "shipping_company1": "l.shipping_company",
    "shipping_date1": "l.shipping_date",
    "shipping_multiselect1": "l.shipping_multiselect",
    "shipping_select1": "l.shipping_select",
    "shipping_state1": "l.shipping_state",
    "skype1": "l.skype",
    "state1": "l.state",
    "title1": "l.title",
    "twitter1": "l.twitter",
    "website1": "l.website",
    "zipcode1": "l.zipcode",
    "new_boolean1": "l.new_boolean",
    "textarea_test1": "l.textarea_test"
  },
  "limit": 5,
  "page": 3,
  "dateFrom": "2017-01-01T00:00:00+00:00",
  "dateTo": "2018-10-24T11:55:29+00:00",
}
```
Get an individual report by ID.

#### HTTP Request

`GET /reports/ID`

Or define query params like this:

`GET /reports/3?dateFrom=2017-01-01&dateTo=2018-01-01&limit=5&page=3`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Report Properties**

Name|Type|Description
----|----|-----------
totalResults|int|Amount of results in the defined date range. Default date range is from 30 days ago to now
data|array|Holds rows of the report specific to each report data type and selected columns
dataColumns|array|Array of supported column names for the report data type
limit|int|Currently applied limit
page|int|Currently applied page
dateFrom|datetime|Currently applied date from filter
dateTo|datetime|Currently applied date to filter

### List Reports

```php
<?php

//...
$reports = $reportApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);
```
```json
{  
  "total":8,
  "reports":[  
    {  
      "id":1,
      "name":"Contacts",
      "descriptionn":"lists all contacts",
      "system": false,
      "isScheduled": false,
      "source": "leads",
      "columns": [
        "l.id",
        "l.email",
        "l.firstname",
        "l.lastname"
      ],
      "filters": [],
      "tableOrder": [],
      "graphs": [],
      "groupBy": [],
      "settings": {
        "showGraphsAboveTable": 0,
        "showDynamicFilters": 0,
        "hideDateRangeFilter": 0
      },
      "aggregators": [],
      "scheduleUnit": null,
      "toAddress": null,
      "scheduleDay": null,
      "scheduleMonthFrequency": null
    },
    [...]
  ]
}
```
Returns a list of contact reports available to the user. This list is not filterable.

#### HTTP Request

`GET /reports`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Report Properties**

Name|Type|Description
----|----|-----------
id|int|ID of the report
name|string|The report name
description|string|The report description
system|boolean|If true then the report is visible to all users. If false then only creator can see this report
isScheduled|boolean|Scheduled reports send report emails as the user defines
source|string|Report data source type
columns|array|List of selected columns for this particular report
filters|array|Filters applied on this report
tableOrder|array|Ordering applied on this report
graphs|array|Graphs defined for this report. API won't return graphs
groupBy|array|Group by rules applied for this report
settings|array|Additional settings for the UI layout
aggregators|array|Aggregation rules applied on this report
scheduleUnit|string or null|Unit for the scheduler
toAddress|string or null|Email address for the scheduler
scheduleDay|string or null|Day for the scheduler
scheduleMonthFrequency|string or null|Frequency for the scheduler


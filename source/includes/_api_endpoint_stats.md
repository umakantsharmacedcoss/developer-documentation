## Stats
This endpoint is useful for downloading a full statistical table.

```php
<?php
use Mautic\MauticApi;
use Mautic\Auth\ApiAuth;

// ...
$initAuth = new ApiAuth();
$auth     = $initAuth->newAuth($settings);
$apiUrl   = "https://your-mautic.com";
$api      = new MauticApi();
$statsApi = $api->newApi("stats", $auth, $apiUrl);
```

### Get Available Stat Tables
```php
<?php

//...
$tables = $statsApi->get();
```
```json
{  
  "availableTables":[  
    "asset_downloads",
    "audit_log",
    "campaign_lead_event_log",
    "email_stats",
    "email_stats_devices",
    "focus_stats",
    "form_submissions",
    "lead_companies_change_log",
    "lead_points_change_log",
    "lead_stages_change_log",
    "page_hits",
    "point_lead_action_log",
    "point_lead_event_log",
    "push_notification_stats",
    "sms_message_stats",
    "stage_lead_action_log",
    "video_hits",
    "webhook_logs"
  ],
  "stats":[]
}
```

#### HTTP Request

`GET /stats`

#### Response

`Expected Response Code: 200`

See JSON code example.

**Stats Properties**

Name|Type|Description
----|----|-----------
availableTables|array|List of available tables which can be used in this endpoint
stats|array|An empty array of stats, because no table was defined

### Get Stats from a table
```php
<?php
// Example setup variables:
$table = 'asset_downloads'; 
$start = 0;
$limit = 50;
$order = array(
    array(
      'col' => 'id',
      'dir' => 'asc'
    )
);
$where = array(
    array(
      'col' => 'id',
      'expr' => 'gt',
      'val' => 3,
    )
);

$stats = $statsApi->get($table, $start, $limit, $order, $where);
```
```json
{  
  "stats":[  
    {  
      "id":"1",
      "asset_id":"1",
      "ip_id":"1",
      "lead_id":"31",
      "date_download":"2016-06-30 08:51:22",
      "code":"200",
      "tracking_id":"b3259e7709f35b7428b7bffcbb3d1d713ac1526c"
    },
    [...]
  ]
}
```

#### HTTP Request

`GET /stats`

**Request Properties**

Name|Type|Description
----|----|-----------
table|string|Stat table name
start|int|Which row to start on
limit|int|How many rows to return
order|array|An array of arrays which contain ordering (example above)
where|array|An array of arrays which contain where conditions (example above). As the `expr` param can be used most of the methods from [DBAL Doctrine where methods](http://www.doctrine-project.org/api/dbal/2.3/class-Doctrine.DBAL.Query.Expression.ExpressionBuilder.html).

#### Response

`Expected Response Code: 200`

See JSON code example.

**Stats Properties**

Different for every table. It simply returns rows or requested table.

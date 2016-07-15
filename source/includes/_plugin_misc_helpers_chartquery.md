#### ChartQuery and Graphs

There are several classes available to assist with generating chart data.

```php
<?php
use Mautic\CoreBundle\Helper\Chart\LineChart;
use Mautic\CoreBundle\Helper\Chart\ChartQuery;

$chart = new LineChart($unit, $dateFrom, $dateTo, $dateFormat);
$query = new ChartQuery($this->em->getConnection(), $dateFrom, $dateTo);
$q     = $query->prepareTimeDataQuery('lead_points_change_log', 'date_added', $filter);
$data = $query->loadAndBuildTimeData($q);
$chart->setDataset($this->translator->trans('mautic.point.changes'), $data);
$data = $chart->render();
```
<div class="clear-right"></div>

##### ChartQuery

##### LineChart

##### PieChart

##### BarChart
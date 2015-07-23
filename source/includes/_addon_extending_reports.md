### Extending Reports

```php
<?php
// addons\HelloWorldBundle\EventListener\ReportSubscriber

namespace MauticAddon\HelloWorldBundle\EventListener;

use Mautic\CoreBundle\EventListener\CommonSubscriber;
use Mautic\CoreBundle\Helper\GraphHelper;
use Mautic\ReportBundle\Event\ReportBuilderEvent;
use Mautic\ReportBundle\Event\ReportGeneratorEvent;
use Mautic\ReportBundle\Event\ReportGraphEvent;
use Mautic\ReportBundle\ReportEvents;

/**
 * Class ReportSubscriber
 */
class ReportSubscriber extends CommonSubscriber
{

    /**
     * @return array
     */
    static public function getSubscribedEvents ()
    {
        return array(
            ReportEvents::REPORT_ON_BUILD          => array('onReportBuilder', 0),
            ReportEvents::REPORT_ON_GENERATE       => array('onReportGenerate', 0),
            ReportEvents::REPORT_ON_GRAPH_GENERATE => array('onReportGraphGenerate', 0)
        );
    }

    /**
     * Add available tables, columns, and graphs to the report builder lookup
     *
     * @param ReportBuilderEvent $event
     *
     * @return void
     */
    public function onReportBuilder (ReportBuilderEvent $event)
    {
        // Use checkContext() to determine if the report being requested is this report
        if ($event->checkContext(array('worlds'))) {
            // Define the columns that are available to the report.
            $prefix        = 'w.';
            $columns = array(
                $prefix . 'visit_count' => array(
                    'label' => 'mautic.hellobundle.report.visit_count',
                    'type'  => 'int'
                )
            );
            
             // Several helper functions are available to append common columns such as categories, publish state fields, lead, etc.  Refer to the ReportBuilderEvent class for more details.
            $columns       = array_merge($columns, $event->getStandardColumns($prefix), $event->getCategoryColumns());
            
            // Add the table to the list
            $event->addTable('worlds',
                array(
                    'display_name' => 'mautic.helloworld.worlds',
                    'columns'      => $columns
                )
            );

            // Register available graphs; can use line, pie, or table
            $event->addGraph('worlds', 'line', 'mautic.hellobundle.graph.line.visits');
        }
    }

    /**
     * Initialize the QueryBuilder object used to generate the report's data. 
     * This should use Doctrine's DBAL layer, not the ORM so be sure to use 
     * the real schema column names (not the ORM property names) and the
     * MAUTIC_TABLE_PREFIX constant.
     *
     * @param ReportGeneratorEvent $event
     *
     * @return void
     */
    public function onReportGenerate (ReportGeneratorEvent $event)
    {
        $context = $event->getContext();
        if ($context == 'worlds') {
            $qb = $this->factory->getEntityManager()->getConnection()->createQueryBuilder();

            $qb->from(MAUTIC_TABLE_PREFIX . 'worlds', 'w');
            $event->addCategoryLeftJoin($qb, 'w');

            $event->setQueryBuilder($qb);
        }
    }

    /**
     * Generate the graphs
     *
     * @param ReportGraphEvent $event
     *
     * @return void
     */
    public function onReportGraphGenerate (ReportGraphEvent $event)
    {
        if (!$event->checkContext('worlds')) {
            return;
        }

        $graphs   = $event->getRequestedGraphs();
        $qb       = $event->getQueryBuilder();
        $repo     = $this->factory->getEntityManager()->getRepository('HelloWorldBundle:World');

        foreach ($graphs as $graph) {
            $options      = $event->getOptions($graph);
            $queryBuilder = clone $qb;

            switch ($graph) {
                case 'mautic.hellobundle.graph.line.visits':
                    // Generate data for Stats line graph
                    $unit   = 'D';
                    $amount = 30;

                    if (isset($options['amount'])) {
                        $amount = $options['amount'];
                    }

                    if (isset($options['unit'])) {
                        $unit = $options['unit'];
                    }

                    // Prepare the line graph
                    $graphStats = GraphHelper::prepareDatetimeLineGraphData($amount, $unit, array('visits'));

                    // Get counts from repo
                    $visitStats = $repo->getVisitStats($graphStats['fromDate']);

                    $graphStats  = GraphHelper::mergeLineGraphData($graphStats, $visitStats, $unit,

                    $graphData['data']      = $graphStats;
                    $graphData['name']      = $graph;
                    $graphData['iconClass'] = 'fa-tachometer';
                    $event->setGraph($graph, $graphStats);
                    break;
            }
        }
    }
}
```

Adding and rendering custom reports are done by listening to the `\Mautic\ReportBundle\ReportEvents::REPORT_ON_BUILD`, `ReportEvents::REPORT_ON_GENERATE` and `ReportEvents::REPORT_ON_GRAPH_GENERATE` events.

#### Defining the Report

Defining the report is done through the `ReportEvents::REPORT_ON_BUILD` event. This is where the addon will define the context of the report, available columns for table data, and available graphs. See the code example's `onReportBuilder` for details.

#### Generate the QueryBuilder

The `ReportEvents::REPORT_ON_GENERATE` event is dispatched when a report is to be generated and displayed. In this function, the addon should define the QueryBuilder object used to generate the table data. 

Use `$event->checkContext()` to determine if the report requested is the subscribers report.

<aside class="notice">
Note that the <code>ReportEvents::REPORT_ON_GENERATE</code> event should use Doctrine's DBAL layer QueryBuilder obtained via <code>$qb = $this->factory->getEntityManager()->getConnection()->createQueryBuilder();</code>.
</aside>

There are a number of helper functions to append joins for commonly used relationships such as category, leads, ip address, etc.  Refer to the ReportGeneratorEvent class for more details.
   
#### Graphs

Graphs are generated using `ReportEvents::REPORT_ON_GRAPH_GENERATE` event. The listener should check the context then generate and set the graph data.  The `Mautic\CoreBundle\Helper\GraphHelper()` class can be used to assist in formatting the data for the graph.
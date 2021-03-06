# YACronParser
This class can parse a cron string and get the last run time.

It can take a string with the list schedule definitions in cron format for several tasks and extracts the last time a task should have been run.

The class supports the numeric format for the definition of each task minute, hour, day, month and year, as well the symbolic format for the schedule names.

Usage

```php
foreach ( self::getCronTabList () as $sheduleItem ) {

      $storedLastRunTime = strtotime( ( $sheduleItem->lastRun == '' ) ? $sheduleItem->start : $sheduleItem->lastRun );
      $previousCalculatedRunTime = YACronParser::lastRun( $sheduleItem->cron );

      // This looks at when the item had run. If the stored value is less than
      // the calculated value means that we have past a run period. So need to run
      if ( $storedLastRunTime <  $previousCalculatedRunTime ) {

          // Update the run time to now
          $sheduleItem->lastRun = strftime ( '%Y-%m-%d %H:%M', $previousCalculatedRunTime );
          $sheduleItem->save ();

          // Do the work to run the cron job here
          // ..................
      }
}
```

Test cases:

```php
date_default_timezone_set ( 'Australia/Brisbane' );
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '5/15 0 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0/15 * * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0 2 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '* 11-20 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0/15 * * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '* * * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '* 12-21 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0 2 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '1-59/2 14-23 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0-58/2 14-23 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '* * * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '* 22-23 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '1-2 22-23 * * Monday-Wednesday' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '1,2,3,4,10-20 11-20 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0 */4 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0 0/4 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '0 2/3 * * *' ) ) . '<br/>' . "\n";
echo 'LastRun Date: ' . date ( 'Y-m-d H:i', CronParser::lastRun( '30-50/3 * * * *' ) ) . '<br/>' . "\n";
```

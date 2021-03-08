# mysql_sys.x-host_summary_by_file_io

SELECT 
    IF((`performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST` IS NULL),
        'background',
        `performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST`) AS `host`,
    SUM(`performance_schema`.`events_waits_summary_by_host_by_event_name`.`COUNT_STAR`) AS `ios`,
    SUM(`performance_schema`.`events_waits_summary_by_host_by_event_name`.`SUM_TIMER_WAIT`) AS `io_latency`
FROM
    `performance_schema`.`events_waits_summary_by_host_by_event_name`
WHERE
    (`performance_schema`.`events_waits_summary_by_host_by_event_name`.`EVENT_NAME` LIKE 'wait/io/file/%')
GROUP BY IF((`performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST` IS NULL),
    'background',
    `performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST`)
ORDER BY SUM(`performance_schema`.`events_waits_summary_by_host_by_event_name`.`SUM_TIMER_WAIT`) DESC

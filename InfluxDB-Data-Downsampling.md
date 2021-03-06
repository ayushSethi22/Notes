# InfluxDB Data Downsampling

### Retention policy

    drop retention policy "default" on "monitoring"
    
    ALTER RETENTION POLICY autogen ON "monitoring" DURATION 2w REPLICATION 1 SHARD DURATION 0h5m0s DEFAULT
    
    #create_retention_policy(name, database, duration, replication, default = false, shard_duration: nil)
    CREATE RETENTION POLICY "last-4-weeks" ON "monitoring" DURATION 4w REPLICATION 1 SHARD DURATION 0h5m0s
    
    show retention policies
    
### Continuous query
    
    CREATE CONTINUOUS QUERY "cq_5m" ON "monitoring" BEGIN SELECT mean(*) INTO "monitoring"."last-4-weeks".:MEASUREMENT FROM "monitoring"."autogen"./.*/ GROUP BY time(5m),* END
    
    show continuous queries
    
### See data in a given retention policy of a measurement
    
    select * from "monitoring"."autogen"."cpu" limit 5
    
    select * from "monitoring"."last-4-weeks"."cpu" limit 5
    
### Grafana dashboard query
    
    SELECT last(/^(mean_)?rate$/) FROM "$retention_policy"."haproxy" WHERE "host" =~ /^$server$/ AND $timeFilter GROUP BY time($interval)

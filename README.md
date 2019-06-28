# Synthetic CDC Data Generator

This is a simple routine to generate random data with a configurable number or records, key fields and non key fields to be used to create synthetic data for source change data capture (CDC) processing.  The output includes an initial directory containing CSV files representing an initial data load, and an incremental directory containing CSV files representing incremental data.
## Usage

Arguments by position include:

 - `no_init_recs` (the number of initial records to generate)
 - `no_incr_recs`    (the number of incremental records on the second run - should be >=    no_init_recs) 
 - `no_keys` (number of key columns in the dataset – keys    are generated as UUIDs) 
 - `no_nonkeys` (number of non-key columns in the    dataset – nonkey values are generated as random numbers) 
 - `pct_del`    (percentage of initial records deleted on the second run - between  0.0 and 1.0) 
 - `pct_upd` (percentage of initial records updated on the second run - between 0.0 and 1.0) 
 - `pct_unchanged` (percentage of records unchanged on the second run - between 0.0 and 1.0)    
 - `initial_output` (folder for initial output in CSV format)
 - `incremental_output` (folder for incremental output in CSV format)

> **NOTE :** `pct_del` + `pct_upd` + `pct_unchanged` must equal 1.0
### Example
The following example creates two datasets each with 1,000,000 records with 2 key columns and 3 non key columns:

    $ spark-submit synthetic-cdc-data-generator.py 1000000 1000000 2 3 0.2 0.4 0.4 data/day1 data/day2
Example output from the day1 run for the above configuration (when inspected as a Spark Dataframe using `spark.read.csv()`) would look like this:

    +--------------------+--------------------+------------------+------------------+------------------+
    |                 _c0|                 _c1|               _c2|               _c3|               _c4|
    +--------------------+--------------------+------------------+------------------+------------------+
    |b5ae7288-9517-11e...|b5ae7289-9517-11e...|0.3773463953004985|0.7573784656097071|0.7344416545822182|
    |b5ae728a-9517-11e...|b5ae728b-9517-11e...|0.9418408807626378|0.3253174416207214|0.8719029391821419|
    +--------------------+--------------------+------------------+------------------+------------------+
Note that this routine can be run subsequent times producing different key and non key values each time, as the keys are UUIDs and the values are random numbers.
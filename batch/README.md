# Batch processing
The pipelined we decided to use was sqoop for the measurements table and spark/jdbc for the smaller tables. Sqooping the measurements table was necessary for parallel ingestion of 500m records. Spark, through the sqlContext read method, does not ingest in parallel but for smaller tables this was satisfactory. This gain us the ability to sqoop only once and immediately join the tables in spark. 

# Spark applications
We implemented 2 versions of the spark application, python and scala.
* [Python](batch.py)
* Scala see spark project [here](../spark)

Knowing python is advantageous in engagements where there is NOT a way to compile scala code on an edgenode.

# sqoop
```
sqoop import \
  --num-mappers 9 \
  --connect "jdbc:oracle:thin:@gravity.cghfmcr8k3ia.us-west-2.rds.amazonaws.com:15210:gravity" \
  --username=gravity \
  --password=bootcamp \
  --direct \
  --fetch-size 10000 \
  --table MEASUREMENTS \
  --hive-import \
  --create-hive-table \
  --hive-database gravity2 \
  --hive-overwrite \
  --hive-table measurements_raw
```

## Redset
Redset is a dataset containing three months worth of user query metadata that
ran on a selected sample of instances in the Amazon Redshift fleet. We provide
query metadata for 200 provisioned and serverless instances each.

## Security
See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License
Redset © 2024 by Amazon is licensed under Creative Commons
Attribution-NonCommercial 4.0 International.

## Download
Folder structure:
* s3://redshift-downloads/redset
  * README
  * LICENSE
  * provisioned/
    * full.parquet
    * sample_0.01.parquet (1% uniform random data sample)
    * sample_0.001.parquet (0.1% uniform random data sample)
    * parts/
      * One individual `<id>.parquet` file per cluster
  * serverless/
    * full.parquet
    * sample_0.01.parquet (1% uniform random sample)
    * sample_0.001.parquet (0.1% uniform random data sample)
    * parts/
      * One individual `<id>.parquet` file per cluster

You can either download files using their http link, e.g.,
https://s3.amazonaws.com/redshift-downloads/redset/LICENSE
Or interact with the s3 bucket using the [AWS CLI](https://aws.amazon.com/cli/).
For example, to download the full serverless dataset you can run:
```
aws s3 cp --no-sign-request s3://redshift-downloads/redset/serverless/full.parquet .
```

## Schema
| Column | Name	Description	|
| ------ | ---------------- |
| instance_id |	Uniquely identifies a redshift cluster |
| cluster_size | Size of the cluster (only available for provisioned) |
| user_id |	Identifies the user that issued the query |
| database_id |	Identifies the database that was queried |
| query_id | Unique per instance |
| arrival_timestamp | Timestamp when the query arrived on the system |
| compile_duration_ms |	Time the query spent compiling in milliseconds |
| queue_duration_ms | Time the query spent queueing in milliseconds |
| execution_duration_ms | Time the query spent executing in milliseconds |
| feature_fingerprint |	Hash value of the query fingerprint. A proxy for query-likeness, though not based on text. Will overestimate repetition. |
| was_aborted |	Whether the query was aborted during its lifetime |
| was_cached | Whether the query was answered from result cache |
| cache_source_query_id | If query was answered from result cache, this is the query id for the query which populated the cache |
| query_type | Type of query, e.g.., `select`, `copy`, ... |
| num_permanent_tables_accessed | Number of permanent table accesses by the query (regular database table) |
| num_external_tables_accessed | Number of [external tables](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_TABLE.html) accessed by the query |
| num_system_tables_accessed | Number of [system tables](https://docs.aws.amazon.com/redshift/latest/dg/cm_chap_system-tables.html) accessed by the query |
| read_table_ids | Comma separated list of unique permanent table ids read by the query |
| write_table_ids |	Comma separated list of unique table ids written to by the query |
| mbytes_scanned | Total number of megabytes scanned by the query |
| mbytes_spilled | Total number of megabytes spilled by the query |
| num_joins | Number of joins in the query plan |
| num_scans | Number of scans in the query plan |
| num_aggregations | Number of aggregations in the query plan |

## Citation
```
@Inproceedings{Renen2024,
  author = {Alexander van Renen and Dominik Horn and Pascal Pfeil and Kapil Eknath Vaidya and Wenjian Dong and Murali Narayanaswamy and Zhengchun Liu and Gaurav Saxena and Andreas Kipf and Tim Kraska},
  title = {Why TPC is not enough: An analysis of the Amazon Redshift fleet},
  year = {2024},
  url = {https://www.amazon.science/publications/why-tpc-is-not-enough-an-analysis-of-the-amazon-redshift-fleet},
  booktitle = {VLDB 2024},
}
```

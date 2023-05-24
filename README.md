# Codebase
  The solution is provided as an interactive Jupyter notebook
  - Notebook      : token_volume.ipynb


# Input Files
This repository does not contains input data files. The total input data size is too big (+60GB) to host in the repository. \
To run this notebook, please copy all input data into the subdirectory "./data". \
You can use one of following *awscli* commands in Linux.
  - aws  s3  sync  s3://rwa-xyz-recruiting-data/eth_logs_decoded/*  <this_repo_dir>/data/
  - aws  s3  cp    s3://rwa-xyz-recruiting-data/eth_logs_decoded/*  <this_repo_dir>/data/

*awscli* download and install instruction:
  - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions


#  Output Files
Output files are produced into the subdirectory "./output". \
There are six csv files with the following naming convention.
  - \<period\>_max_\<measure\>.csv

where,
  - period  : one of *"monthly"*, *"weekly"*, and *"daily"*
  - measure : one of *"transfer\_count"* and *"transfer_amount"*

   ```
	output/
	├── monthly_max_transfer_count.csv
	├── monthly_max_transfer_amount.csv
	├── weekly_max_transfer_count.csv
	├── weekly_max_transfer_amount.csv
	├── daily_max_transfer_count.csv
	└── daily_max_transfer_amount.csv
```


# Future Improvement
* Polars/Pyarrow Implemetation \
  Initially, the solution was implemeted by Polars dataframe (https://www.pola.rs/).
  It delivered 4 times faster than Pandas dataframe implementation.
  However,it produced a lot of numeric range limit overflows during (very) long hexadecimal string conversion into integer values.
  It makes me stick to the Pandas dataframe solution (until now). It requries more time to investigate and resolve this issue.
  As alternative to Polars, we can also consider "Pyarrow".
* Direct Data Import from AWS S3 Buckets \
  *Polars*, *Pyarrow*, and *s3fs* enable to load Parquet files stored in AWS S3 buckets directly or indirectly.
  It makes this solution more flexible dealing with multiple data storage sources. Similar steps can be done over other cloud storages provided by *GCP*, *Azure*, and *OCI*


# Data Access through APIs
  Output data can be accessible through a few diffent ways including (i) REST APIs, (ii) WebSocket, and (iii) Kafka message/streaming channels.
  To minimize the latency between the data and REST API & WebSocket servers (e.g., Nginx Apache, and FastAPI), it is recommended to host output data in Redis in-memory cache.
  Different time-resolution output data access can be designed as (i) multipe *"routes"* in REST API servers, (ii) WebSocket *"connections"* in WebSocket servers, and  (iii) message *"topics"* in kafka clusters.

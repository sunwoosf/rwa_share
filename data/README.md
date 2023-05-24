Copy (all) input data into this directory before running the notebook. \
You can use one of following awscli commands in Linux.

* aws s3 sync s3://rwa-xyz-recruiting-data/eth_logs_decoded/* <this_repo_dir>/data/
* aws s3 cp s3://rwa-xyz-recruiting-data/eth_logs_decoded/* <this_repo_dir>/data/

*awscli* download and install instruction:
- https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#getting-started-install-instructions

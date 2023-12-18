# Youtube-ETL-Project
Extract raw youtube video data and then get useful insights from it

Used the below architecture to complete the project
![image](https://github.com/ankitgupta14011998/Youtube-ETL-Project-/assets/32798626/a7b77613-9312-47c0-8d4e-9c9febef4c11)

Video Links
1: https://youtu.be/yZKJFKu49Dk?si=0SB_4hEzyZD3f4_H \n

2: https://www.youtube.com/watch?v=qFaaKme5eDE&list=PLBJe2dFI4sguF2nU6Z3Od7BX8eALZN3mU&index=2

Kaggle data source link:
https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbWN3ckR3SUc3X0xLTm9WTk1NV29yZFpnTXBsUXxBQ3Jtc0tuQy1RVEZJamxVQ1FsNkxRcmY4Y2E2c1V4cEduV09WVjgzLU8wb1lfbWdNTkdoSHZqUnFteGtCUzFlOG9GOVMteDZwWFEtdTZzNFRFTHoxWlp4OWNwN2Y5X0ZWLXVEandzaWNXaWZ5QVZvTmlqQlZrUQ&q=https%3A%2F%2Fwww.kaggle.com%2Fdatasnaek%2Fyoutube-new&v=yZKJFKu49Dk

Step 1: Data ingestion

* Used **aws CLI** to upload raw data into **S3 buckets**. We have 2 types of data statistics_reference_data.json and statistics_data.csv for various regions.
* create necessary users , roles and permission using **AWS IAM**

Step 2: Data processing 

* Used **AWS glue crawler** to analyze our data and build a data catalog. This will create a structured tabular data.
* We then used **AWS Athena** to query these data. The Tabular data in **AWS glue** might not be in correct format for sql querying.
* We then create a **lambda** function to perform some preprocessing over raw table to create cleansed table .This will create a parquet file in S3. This table will be used by athena to perform queries.
* To perform this preprocessing on all the files that comes into the raw bucket , we add triggers in lambda function and store the output result in S3 bucket.
* For statistics_reference_data also we can first use crawler to generate tables and query using Athena.
* We can create an **AWS ETL job** over these reference_data to perform any necessary pre-processing on the CSV files and created cleansed table.
* To get the data for analytics we can apply our business logic(In our case we have done inner join on cleansed reference_data and cleansed data on category_id. To automate this process we have used AWS ETL job and stored our final data in S3 bucket and also created cleaned table.

Step 3: Data visualization

* This data can further be used in **Quicksight** for Data reporting and visualization.


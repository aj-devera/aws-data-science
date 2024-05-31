# aws-data-science
End-to-end data science project in AWS

Services:
1. S3
2. Glue
3. Athena
4. SageMaker
5. VPC
6. IAM

## Creating and Placing the Data in S3 Bucket
Create the S3 bucket. The data used can be downloaded here: [https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data](https://www.kaggle.com/datasets/varpit94/bitcoin-data-updated-till-26jun2021)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/ed2bb311-f8a7-42b5-adde-450606d845b9)

## Glue
#### Glue Crawler, Glue Database, Table
Set up the Glue Crawler

![image](https://github.com/aj-devera/aws-data-science/assets/87835106/08253529-ad33-4aa8-b77e-2dfe0481e7fc)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/b6b03a69-379c-4658-8fce-919a141e6ea4)

For simplicity, Glue role has the AWSGlueConsoleFullAccess, AWSGlueServiceRole, and AmazonS3FullAccess policy attached to it.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/042c231e-c45a-4247-9650-4a5c7c5260d5)

Specify the target database. If no database yet, create the database. All settings are in default and Frequency is set to "on-demand". 
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/509152df-f8d0-4a72-90ce-7bcae6638f63)

After reviewing the Glue Crawler settings, create the crawler.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/0203fe7b-4e65-41bc-a34f-38feb42bf573)

If we click the Run Crawler, this should fetch the CSV data from the S3 bucket and export it on the specified table in Glue Database. This can be consumed by Athena for querying.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/8321685c-dcb9-4fc6-b60b-dbd4b2468b5f)

## Athena
In Athena, specify the S3 bucket where the saved queries will be saved. In this demo, we've created a new S3 bucket where these saved queries will be uploaded.

![image](https://github.com/aj-devera/aws-data-science/assets/87835106/77722f1c-189d-402e-987a-f9b0916fd83c)

Going back to the Athena Query Editor, select the Data Source and Database from Glue. We should see the table created by our Glue Crawler earlier.

![image](https://github.com/aj-devera/aws-data-science/assets/87835106/a185d2ef-476a-4abf-a602-b52f30173f4a)

Sample SQL query in Athena where it will get 50 data from the table from all columns. We can save this query and upload to the specified S3 bucket earlier by clicking the 3 dots beside the "Query 1" name
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/6dad3bca-c36e-4850-be6d-5f80ee87eb71)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/19bde39d-04ae-4178-a2fa-da0efbd7de19)

In the S3 bucket set in Athena (ajgithub-athena-bucket), we can see our saved queries that other services (e.g. SageMaker) can use.

![image](https://github.com/aj-devera/aws-data-science/assets/87835106/67833225-7714-4d71-b322-8d6493c8c76b)

## SageMaker
<b>NOTE</b>: The dataset used here will be another dataset from IMDB and the ML Application will be Sentiment Analysis. Dataset will be uploaded in ajgithub-bitcoin-data 
Reference: https://github.com/aj-devera/ML-Projects/blob/main/Application%20Sentiment%20Analysis.ipynb

Create a notebook instance. For this demo, the minimum value of notebook instance type and volume size will be used.

![image](https://github.com/aj-devera/aws-data-science/assets/87835106/39046a56-89b0-4b83-9c07-ea96495e494a)

Create an IAM Role to be attached in the SageMaker notebook and specify the ARNs of the Athena saved queries and Raw data S3 buckets. A KMS Key can be attached for additional security but we'll leave it out for now. 

![image](https://github.com/aj-devera/aws-data-science/assets/87835106/c419613b-aa12-44be-ad2c-392ec212ef3a)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/a8dd94d7-1123-473d-bfd3-8bd0a1476e74)

As for the Network Components, the instance will be placed in a public subnet with a default security group that has 0.0.0.0/0 inbound and outbound rules. This is not the best practice but for testing purposes, this will be used.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/bfb5a80c-b6c1-493a-be33-2848a80500b0)

After the SageMaker Notebook Instance has been created, choose Open JupyterLab and create the code.

First, install other packages if needed then import relevant packages.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/40368a12-7c61-4a11-a1f8-de2107751f90)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/6dd8f3e0-242e-4959-9ec8-03bd0eb1d8b7)

Check if notebook instance can read the S3 buckets. Read the dataset from the S3 bucket.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/0842e196-af94-4b27-86ba-366ff1c1f49c)

You can check https://github.com/aj-devera/ML-Projects/blob/main/Application%20Sentiment%20Analysis.ipynb for the succeeding code runs.
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/0b6d8910-0e16-4e9e-a7e5-d7137a5f123f)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/a1b1fe6a-5713-4045-bba9-d7504c1aab4e)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/eff4a231-a206-4793-a42e-ed548ab228f4)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/5d7ea1a2-08e6-41ef-8a45-aba150796ff2)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/d3021640-a97d-4eac-9c7b-a35159c0e16b)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/40b53594-9c41-49fe-a5ee-321904a3b97e)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/606d5c7f-2e99-4536-9060-540997136c99)
![image](https://github.com/aj-devera/aws-data-science/assets/87835106/29dda2ba-fc17-4971-b02f-fa8f6e0a37d6)






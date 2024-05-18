# 💭 Athena DynamoDB Connector Cross-Account

### Deploy a Data Source Connector

> **ℹ️ NOTE**
>
> The Data Source is not yet supported by the CDK at the moment. To set up an Amazon DynamoDB Connector, we'll need to use the AWS Management Console to configure it.
>
> **Account A**
> * Create an S3 Bucket that will hold Athena's query results (e.g. `athena-dynamodb-queries-a`)
> * Create an S3 Bucket that will store the data that exceeds the Lambda function limits (e.g. `ds-spill-bucket`)
> * Make sure that the DynamoDB Table is already created so we can add it as the Data Source.
>
> **Account B**
> * Create an S3 Bucket that will hold the Athena's query results (e.g. `athena-dynamodb-queries-b`)

#### 📝 Steps

1. Go to Amazon Athena Console → Administration → Data sources → **Create data source**
2. Choose **Amazon DynamoDB** as your data source → **Next**
3. Enter the Data Source Details

    a. Data Source Name (e.g. `data_source_table`)

    b. If you haven't deployed yet the AWS Lambda function to connect to the Data Source, you can **Create Lambda function**
      * **SpillBucket**: It is a required field that will store the data that exceeds Lambda function limits (e.g. `ds-spill-bucket`)
      * **AthenaCatalogName**: It will serve as the Lambda Function name (e.g. `athena-dynamodb-lambda-connector-account-a`)
      * **LambdaMemory**: It is the lambda memory, the default value is `3008` which is the max memory
      * **LambdaTimeout**: It is the lambda invocation runtime in seconds, the default value is `900` (15 minutes)
      * Make sure to check the checkbox to acknowledge that the app will create a custom IAM role and resource policies
      * **Deploy**

4. After you have finished filling up the Data Source details, **Create data source**
5. Going to the **Query Editor** and if it is your first query, you need to set up an S3 Bucket that will be holding the query result of Athena
   * Amazon Athena Console → **Query editor** → **Settings** → **Manage** → **Browse S3** → `athena-dynamodb-queries-a` → **Save**

### Cross-Account Amazon DynamoDB Tables using Amazon Athena

#### Set up IAM Permissions for Cross-Account on Account A
1. On the S3 spill bucket (of the Lambda function), grant `GetObject` and `ListBucket` permissions to *Account B*

    `ds-spill-bucket` → **Permissions** → **Bucket policy** → **Edit**

    ```json
    {
      "Version": "2008-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "AWS": "ACCOUNT_B_ID"
          },
          "Action": ["s3:GetObject", "s3:ListBucket"],
          "Resource": [
            "arn:aws:s3:::ds-spill-bucket",
            "arn:aws:s3:::ds-spill-bucket/*"
          ]
        }
      ]
    }
    ```

2. Grant `InvokeFunction` on Lambda function `athena-dynamodb-lambda-connector-account-a` to *Account B*

    a. Go to `athena-dynamodb-lambda-connector-account-a` → **Configuration** → **Permissions** → **Resource-based policy statements** → **Add permissions**

    b. Select the **AWS account** and enter the Policy statement details
      * **Statement ID**: `athena-cross-account-access`
      * **Principal**: `Account B ID`
      * **Action**: `lambda:InvokeFunction`

#### Set up IAM Permissions for Cross-Account on Account B
1. Create an IAM Role called `AthenaCrossAccountCreate-Account_A_ID` for Account A to assume
    
    a. Go to **IAM** → **Roles** → **Create role**

    b. Select **AWS account** → **Another AWS Account**
      * Enter the *Account A ID*
    
    c. Click on *Next* until it reaches the **Create role**

2. Go to the newly created IAM Role (`AthenaCrossAccountCreate-Account_A_ID`) and add an ***inline policy***

    a. `AthenaCrossAccountCreate-Account_A_ID` → **Permissions** → **Add permissions** → **Create inline policy**

    b. Create the inline policy using **JSON** → **Review policy** → **Create policy**

      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": "athena:CreateDataCatalog",
            "Resource": "arn:aws:athena:YOUR_AWS_REGION:ACCOUNT_B_ID:datacatalog/*"
          }
        ]
      }
      ```

### Share the Athena Data Source with Account B
1. Sign in to **Account A** and go to **Athena** → **Data sources**

    a. Choose the data source you want to share

    b. Go to the **Share** option in the Actions in the top right corner

2. Enter the ***Account B ID*** to share your data source with Account B and click **Share**

### Test Athena Cross-Account
1. Sign in to Account B
2. In Athena, go to **Data sources**. You will see the data source
3. Go to the **Query editor** and choose the shared data source from *Account A* from the dropdown
    a. If it is your first query, you need to set up an S3 Bucket that will be holding the query result of Athena (e.g. `athena-dynamodb-queries-b`)

## 📋 Related Articles
* [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html)
* [Query cross-account Amazon DynamoDB tables using Amazon Athena Federated Query](https://aws.amazon.com/blogs/big-data/query-cross-account-amazon-dynamodb-tables-using-amazon-athena-federated-query/)
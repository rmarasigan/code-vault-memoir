# 💭 Cross Account AWS Lambda Subscription with an SNS topic

Make sure that:
* The **SNS Topic (*Account A*)** access policy allows Lambda to subscribe to the topic
* The **Lambda Function (*Account B*)** resource policy allows SNS to invoke the function

### Create an SNS Topic
1. Log in to the AWS Management Console, go to the **Amazon SNS**, and then select **Topics**
2. Choose **Create topic**, select **Topic type**, and then enter **Topic name**
3. Choose **Create topic**

### Allow the Lambda Function to perform `Subscribe`
1. Add the following on the SNS Topic **Access Policy** and **Save Changes**

    ```json
    {
      "Effect": "Allow",
      "Principal": "<account_b>",
      "Action": [
        "sns:Subscribe",
        "sns:Receive"
      ],
      "Resource": "<sns_topic_arn>"
    }
    ```

> [!NOTE]
>
> Remember to replace `<account_b>` with your AWS Account number that has the Lambda Function.
> For the `<sns_topic_arn>`, replace it with your Amazon Resource Name (ARN) of the SNS topic.

### Create a Lambda Function
1. Log in to the AWS Management Console, go to **AWS Lambda**, then choose **Create function**
2. Enter the **Function name**
3. For **Execution role**, choose **Create a new role with basic Lambda permissions**

> [!NOTE]
>
> The Lambda Function creates an *execution role* that grants the function permission to upload logs to Amazon CloudWatch.

4. Grant the SNS service principal permission

    * Choose the **Configuration** tab and then choose **Permissions**
    * Scroll down to the **Resource-based policy** section, then **Add Permissions**
    * Select **AWS Service** and choose **SNS** from the dropdown list
      * **Statement ID**: AllowSNSToInvokeFunction
      * **Source ARN**: The SNS ARN created earlier
      * **Action**: `lambda:InvokeFunction`
    * Choose **Save**

    <br />

    > 💡 **HOW TO DO IT VIA CDK?**
    >
    > ```go
    > lambdaFn.AddPermission(jsii.String("AllowSNSToInvokeFunction"),
    >		&awslambda.Permission{
    > 			Action:    jsii.String("lambda:InvokeFunction"),
    > 			SourceArn: jsii.String("arn:aws:sns:<account_a_region>:<account_a_id>:<sns_topic_name>"),
    > 			Principal: awsiam.NewServicePrincipal(jsii.Sprintf("sns.amazonaws.com"), &awsiam.ServicePrincipalOpts{}),
    >		})
    >```

### Subscribe the Lambda function to the SNS topic using AWS CLI

```bash
dev@dev:~/your-project$ aws sns subscribe --topic-arn <sns_arn> \
--protocol lambda --notification-endpoint arn:aws:lambda:<account_b_region>:<account_b_id>:function:<lambda-fn-name> \
--profile <account_b_profile> \
```

> [!NOTE]
>
> Remember to replace `<account_b_*>` with your AWS Account Region, ID and Profile name that has the Lambda Function.
> For the `<sns_arn>`, replace it with your Amazon Resource Name (ARN) of the SNS topic.

## 📋 Related Articles
- [Invoke Lambda using SNS from Outside Account](https://stackoverflow.com/questions/34749310/invoke-lambda-using-sns-from-outside-account)
- [How do I set up a cross-account AWS Lambda subscription with an SNS topic?](https://repost.aws/knowledge-center/sns-with-crossaccount-lambda-subscription)
- [Why do I get an authorization error when I try to subscribe my Lambda function to my Amazon SNS topic?](https://repost.aws/knowledge-center/sns-authorization-error-lambda-function)
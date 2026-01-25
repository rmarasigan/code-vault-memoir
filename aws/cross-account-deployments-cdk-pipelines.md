# 💭 Cross Account Deployments with CDK Pipelines

The AWS CDK Pipeline makes it easy for us developers to deploy our application, even if it consists of multiple stacks deployed in various accounts and regions. As soon as we, developers, start using assets in our CDK application, we need to bootstrap our AWS account.

### 💡 What is Bootstraping?
**Bootstrapping** is the process of preparing an environment for deployment and is a one-time action that you must perform for every environment that you deploy resources into. An **environment** is an (account, region) pair where you want to deploy a CDK stack.

It provisions resources in your environment such as an Amazon Simple Storage Service (Amazon S3) bucket for storing files and AWS Identity and Access Management (IAM) roles that grant permissions needed to perform deployments. These resources get provisioned in an AWS CloudFormation stack, called the *bootstrap stack*. It is usually named **CDKToolkit**.

IAM Roles created:

* **File Publishing Role**
  * Role name: `cdk-hnb659fds-file-publishing-role-${AWS::AccountId}-${AWS::Region}`

* **Image Publishing Role**
  * Role name: `cdk-hnb659fds-image-publishing-role-${AWS::AccountId}-${AWS::Region}`

* **Lookup Role**
  * Role name: `cdk-hnb659fds-lookup-role-${AWS::AccountId}-${AWS::Region}`

* **CloudFormation Execution Role**
  * Role name: `cdk-hnb659fds-cfn-exec-role-${AWS::AccountId}-${AWS::Region}`

* **Deployment Action Role**
  * Role name: `cdk-hnb659fds-deploy-role-${AWS::AccountId}-${AWS::Region}`

### Bootstrap your AWS Accounts

Cross-account deployments mean executing the `cdk deploy` on Account A and wanting to deploy the stack(s) into Account B. This means that Account B needs to trust Account A. The target account needs to be bootstrapped in a way that will allow another account to make use of the local CDK roles.

To bootstrap a different environment for deploying CDK applications into using a Pipeline:

```bash
$ npx cdk bootstrap \
    [--profile account_b_profile] \
    --cloudformation-execution-policies arn:aws:iam::aws:policy/AdministratorAccess \
    --trust <account_a_id> \
    aws://<account_b_id>/<region>
```

> [!NOTE]
>
> `npx` means to use the CDK CLI from the current NPM install. If you are using a global install of the CDK CLI, leave this out.

<br />

```bash
$ cdk bootstrap \
    [--profile account_b_profile] \
    --cloudformation-execution-policies arn:aws:iam::aws:policy/AdministratorAccess \
    --trust <account_a_id> \
    aws://<account_b_id>/<region>
```

## 📋 Related Articles
- [Bootstraping](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html)
- [CDK Environment Bootstrapping](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.pipelines-readme.html#cdk-environment-bootstrapping)
- [Hey CDK, how do cross-account deployments work?](https://garbe.io/blog/2022/08/01/hey-cdk-how-to-cross-account-deployments/)
# 💭 Update the CDK Context

Context in CDK is a combination of `key-value` pairs we can set in our CDK application. These key-value pairs are going to be available at synthesis time, which means that we can use them in our code.

The CDK library uses context to:
* Cache information about the deployment environment.
* Keep track of feature flags. Feature flags provide a way to opt in or out of new functionality that introduces breaking changes, outside of a major CDK version release.

<br />

From the [best practices](https://docs.aws.amazon.com/cdk/v2/guide/best-practices.html#best-practices-apps):

> **Commit cdk.context.json to avoid non-deterministic behavior**
>
> The AWS CDK includes a mechanism called *context providers* to record a snapshot of non-deterministic values. This allows future synthesis operations to produce exactly the same template as they did when first deployed. The only changes in the new template are the changes that you made in your code. When you use a construct's .fromLookup() method, the result of the call is cached in cdk.context.json. You should commit this to version control along with the rest of your code to make sure that future executions of your CDK app use the same value.

### 📝 Steps

1. Build all the executable files that might be needed for *Step 2*.
2. Synthesize your stack with:

    ```bash
    dev@dev:~/your-project$ cdk synth --profile YOUR_AWS_PROFILE_NAME
    ```

    If you open the `cdk.context.json` file in the root of your project, you'll see that the CDK has cached the values that correspond to your stack's configuration.

## 📋 Related Articles
- [How to use Context in AWS CDK](https://bobbyhadz.com/blog/how-to-use-context-aws-cdk)
- [Can the cdk.context.json file be auto-generated for a specific CDK environment?](https://stackoverflow.com/questions/73249557/can-the-cdk-context-json-file-be-auto-generated-for-a-specific-cdk-environment)
- [Best practices for developing and deploying cloud infrastructure with the AWS CDK](https://docs.aws.amazon.com/cdk/v2/guide/best-practices.html)
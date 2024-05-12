# 💭 Update the AWS CDK Stack v1 to v2

1. Update your global CDK version on your local machine

    Before you do this, realize that after you update your AWS CDK version globally on your local machine, ***all your AWS CDK version 1 stack will not be deployable from the update***. You would have to ensure that your code is updated once you run this command:

    ```bash
    dev@dev:~/your-project$ npm install -g aws-cdk
    ```

2. Remove all references to AWS CDK v1 in your NPM packages list

    The goal is to remove all references to AWS CDK V1 within your setup within `package.json`.

    ```json
    {
      "name": "sample-cdk-demo",
      "version": "0.1.0",
      "bin": {
        "gefyra-cdk-demo": "bin/sample-cdk-demo.js"
      },
      "scripts": {
        "build": "tsc",
        "watch": "tsc -w",
        "test": "jest",
        "cdk": "cdk"
      },
      "devDependencies": {
        "@aws-cdk/assert": "^1.90.1", // to be removed
        "@types/jest": "^26.0.20",
        "@types/node": "^10.17.54",
        "aws-cdk": "^1.90.1", // to be removed
        "jest": "^26.4.2",
        "ts-jest": "^26.5.1",
        "ts-node": "^8.1.0",
        "typescript": "^3.9.9"
      },
      "dependencies": {
        "@aws-cdk/aws-lambda": "^1.130.0", // to be removed
        "@aws-cdk/aws-s3": "^1.90.1", // to be removed
        "@aws-cdk/aws-s3-notifications": "^1.130.0", // to be removed
        "@aws-cdk/core": "^1.90.1", // to be removed
        "source-map-support": "^0.5.16"
      }
    }
    ```

    <br />

    Also, delete the `package-lock.json` and remove the `./node_modules` folder in your repository. We will reinstall the base packages without the above libraries.

3. Reinstall the NPM libraries for your stack

    All your TypeScript code within the AWS CDK stack would be broken from here. It is time to switch to the new code.
    * Run `npm install`
      * It will install all the libraries within your new `package.json` file

    * Run `npm install aws-cdk-lib constructs`
      * All CDK references within your code will primarily be sourced from the `aws-cdk-lib` and construct libraries

4. Changing your libraries to fit AWS CDK v2's convention

    * Change the `@aws-cdk/core` into `aws-cdk-lib`
    * Replace all references of `@aws-cdk` to `aws-cdk-lib`
    * Within your constructor, replace the `cdk.Construct` into `Construct` from the newly-added module

      ```ts
      // CDK doesn't contain 'Construct' anymore. From this:
      constructor(scope: cdk.Construct, id: string, props: S3Props)

      // We will use the newly-added module for our construct. To this
      constructor(scope: Construct, id: string, props: S3Props)
      ```

5. Changing all AWS CDK TypeScript test cases to v2

    * Rewrite the `@aws-cdk/assert` to `aws-cdk-lib/assertions`
    * Many of the methods have changed
      * `aws-cdk-lib/assertions` doesn't have `expect`, `matchTemplate`, and `MatchStyle` anymore

6. Remove all deprecated feature flags within `cdk.json`

    * When you run `npm run build && cdk synth`, the compiler will flag all the unsupported feature flags within the `cdk.json`

7. Rerun AWS CDK Bootstrap

    At this point, ensure that `npm run build && cdk synth` runs smoothly. AWS CDK v2 requires us to use a new bootstrap from v1. This means it would reference an S3 bucket to store metadata for your AWS CDK Stack deployments.

    ```bash
    dev@dev:~/your-project$ cdk bootstrap --profile AWS_PROFILE_NAME
    ```

8. Redeploy your stack to ensure your AWS Account is accepting your migration changes within your project

## 📋 Related Articles
- [How to upgrade your AWS CDK Stack to v2](https://gefyra.co/how-to-upgrade-your-aws-cdk-stack-to-version-2-in-typescript/)
* [Migrating from AWS CDK v1 to AWS CDK v2](https://docs.aws.amazon.com/cdk/v2/guide/migrating-v2.html)
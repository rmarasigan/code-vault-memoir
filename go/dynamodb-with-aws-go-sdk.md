# 💭 DynamoDB with AWS Go SDK

1. Create a ".go" file that will contain your logic on how you want to process the data that may be either fetched or retrieved from the DynamoDB or inserted or updated the specific item.

2. For the session to be used, you can use the following:

    * Using `session` package (AWS SDK v1)
      ```go
      sess := session.Must(session.NewSessionWithOptions(session.Options{SharedConfigState: session.SharedConfigEnable}))
      ```

    * Using `config` package (AWS SDK v2)
      ```go
      const Region string = "us-east-1"
      ctx := context.Background()

      cfg, err := config.LoadDefaultConfig(ctx, config.WithRegion(Region))
      if err != nil {
        ...
      }
      ```

3. Export the AWS Environment variables:

    ```bash
    dev@dev:~/your-project$ export AWS_PROFILE=YOUR_AWS_PROFILE
    dev@dev:~/your-project$ export AWS_REGION=YOUR_AWS_REGION
    ```

> [!NOTE]
>
> Make sure to export the necessary AWS environment variables, replacing the `YOUR_AWS_PROFILE` and `YOUR_AWS_REGION` with your actual AWS profile and region.

4. Build and execute the Go file.

      ```bash
      dev@dev:~/your-project$ go build; ./your-project
      ```
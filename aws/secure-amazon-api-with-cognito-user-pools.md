# 💭 Secure Amazon API with Amazon Cognito User Pools

### Amazon Cognito

1. Create a User Pool
    * Enter a Pool Name

2. Create a Resource Server
3. Go to the created User Pool and create an App Client under

    > **ℹ️ NOTE**
    >
    > User Pool = Organization

    <br />

    A bunch of people is registered in your organization and you could have multiple applications that are using the credentials that are stored in that User Pool
    * Enter the App Client Name

4. Go to the App Integration → App client settings
    * Enable the Cognito User Pool under the Enabled Identity Providers
    * Select Client Credentials flow for the Allowed OAuth Flows

5. Create a Domain Name under the App Integration

### Amazon API Gateway

1. Create a REST API
2. Go to Resources → Actions → Create Resource
    * Enter the Resource Name and Resource Path

3. Go to Resources → Actions → Create Method
    * Choose an HTTP Method to use
    * Select Lambda Function as Integration Type
    * Select a Lambda Function that will be triggered by the API Gateway

4. Go to Authorizers → Create New Authorizer
    * Enter the Authorizer Name
    * Choose Cognito as the Authorizer Type
    * Enter the Token Source as Authorization

5. Go to Resources → Method Request → Authorization → Cognito User Pool
    * Select the Cognito user Pool
    * Enter the OAuth Scopes

### Test the API Gateway using Postman

1. Enter the API Gateway Endpoint
2. Set the HTTP Method to call the API Gateway Endpoint

## 📹 Related Tutorial
- [Secure your API Gateway with Amazon Cognito User Pools | Step by Step AWS Tutorial](https://www.youtube.com/watch?v=oFSU6rhFETk)
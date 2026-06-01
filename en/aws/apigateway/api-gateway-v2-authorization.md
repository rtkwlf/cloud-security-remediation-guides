# AWS / API Gateway / API Gateway V2 Authorization

## Quick Info

| | |
|-|-|
| **Plugin Title** | API Gateway V2 Authorization |
| **Cloud** | AWS |
| **Category** | API Gateway |
| **Description** | Ensures that Amazon API Gateway V2 APIs are using authorizer. |
| **More Info** | API Gateway V2 APIs should be configured to use authorizer to enforce security measures and restrict access to API to only authorized users or processes. |
| **AWS Link** | https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-lambda-authorizer.html |
| **Recommended Action** | Modify API Gateway V2 configuration and ensure that appropriate authorizers are set up for each API. |

---

## Introduction

API Gateway V2 HTTP APIs support multiple mechanisms for controlling access: Lambda authorizers, JWT authorizers, and AWS IAM roles. This guide provides steps to create and attach authorizers to protect your API routes and ensure appropriate access control is in place.

## Prerequisites

#### IAM Permissions for API Gateway V2 Management

- User or role must have permissions to create and manage API Gateway V2 authorizers and routes.
- Required IAM actions: `apigatewayv2:CreateAuthorizer`, `apigatewayv2:UpdateRoute`, `apigatewayv2:GetAuthorizers`, and `apigatewayv2:GetApis`.

#### Existing HTTP API

- An HTTP API must already exist in API Gateway V2.
- If no HTTP API exists, create one first using the API Gateway console or CLI.

#### Lambda Function (for Lambda Authorizers)

- If using Lambda authorizers, a Lambda function must already exist that implements the authorization logic.
- The function receives authorization requests from API Gateway and returns an IAM policy or simple response to allow or deny access.

#### Identity Provider Configuration (for JWT Authorizers)

- If using JWT authorizers, an identity provider (such as Amazon Cognito, Auth0, or another OIDC provider) must be configured and accessible.
- You will need the issuer URL and audience identifier from your identity provider to configure the JWT authorizer.

## Remediation Steps

- Select and configure the appropriate authorizer type (JWT or Lambda) based on your authentication needs and existing infrastructure.

> ⚠️ **Warning:** Configuring authorizers on routes will enforce access control. Existing clients without valid credentials or tokens will receive 403 Unauthorized responses. Ensure all legitimate clients are updated with appropriate credentials or tokens before attaching authorizers to production routes.

#### Create JWT Authorizer (Option 1)

1. Sign in to the API Gateway console at https://console.aws.amazon.com/apigateway.
2. Choose an HTTP API. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step1.png"/>
3. In the main navigation pane, choose **Authorization**.
4. Choose the **Manage authorizers** tab.
5. Choose **Create**. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step2.png"/>
6. For **Authorizer type**, choose **JWT**.
7. For **Name**, enter a name for your authorizer (e.g., auth0).
8. For **Identity source**, specify the source of the token (typically `$request.header.Authorization`).
9. For **Issuer URL**, enter your identity provider's issuer URL.
10. For **Audience**, enter your API identifier from your identity provider.
11. Choose **Create**. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step3.png"/>

#### Create Lambda Authorizer (Option 2)

1. Sign in to the API Gateway console at https://console.aws.amazon.com/apigateway.
2. Choose an HTTP API. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step1.png"/>
3. In the main navigation pane, choose **Authorization**.
4. Choose the **Manage authorizers** tab.
5. Choose **Create**. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step2.png"/>
6. For **Authorizer type**, choose **Lambda**.
7. For **Lambda function**, select the AWS Region and enter the function name.
8. For **Identity source**, specify where to extract the identity from (e.g., `$request.header.Authorization`). </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step4.png"/>
9. Ensure **Automatically grant API Gateway invocation permissions on the Lambda function** is enabled.
10. Choose **Create**. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step5.png"/>

#### Attach Authorizers to Routes

1. Sign in to the API Gateway console at https://console.aws.amazon.com/apigateway.
2. Choose an HTTP API. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step1.png"/>
3. In the main navigation pane, choose **Authorization**.
4. Choose a route (method) that requires authorization.
5. Select your JWT or Lambda authorizer from the dropdown menu.
6. Choose **Attach authorizer**. </br> <img src="/resources/aws/apigateway/api-gateway-v2-authorization/step6.png"/>
7. Repeat steps 4–6 for each route that requires authorization.
8. For routes that should use AWS credentials, select **AWS_IAM** from the authorization type dropdown menu and choose **Save** or **Update**.

## Verification Steps

#### Verify Authorizers Exist for API

1. Sign in to the API Gateway console at https://console.aws.amazon.com/apigateway.
2. Choose an HTTP API.
3. In the main navigation pane, choose **Authorization**.
4. Review the **Manage authorizers** tab to see all configured authorizers for the API.
5. Confirm that at least one authorizer is properly configured with the correct type (JWT or Lambda) and identity source.

#### Verify Routes Have Authorization Configured

1. Sign in to the API Gateway console at https://console.aws.amazon.com/apigateway.
2. Choose an HTTP API.
3. In the main navigation pane, choose **Authorization**.
4. For each route, check the authorization type and authorizer assignment.
5. Look for routes with authorization type set to **JWT**, **AWS_IAM**, or **CUSTOM** (Lambda authorizer).
6. Verify that routes have an appropriate authorizer assigned (for JWT and CUSTOM types).
7. Routes with **NONE** authorization type are open access and do not require authorization.
8. Ensure all routes that should be protected have an appropriate authorizer configured.
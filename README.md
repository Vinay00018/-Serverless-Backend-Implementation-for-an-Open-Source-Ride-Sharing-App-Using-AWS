# -Serverless-Backend-Implementation-for-an-Open-Source-Ride-Sharing-App-Using-AWS

🌟 Introduction
Welcome! In this post, I’ll guide you through how I built a scalable, secure, serverless ride-sharing web app using popular AWS services. From linking my code with GitHub and Amplify to deploying, securing users with Cognito, managing backend logic with Lambda, and storing data in DynamoDB — this project has it all!

Architecture Overview

Before diving into the implementation, let’s understand the architecture:

<img width="720" height="321" alt="image" src="https://github.com/user-attachments/assets/d8bad80b-f666-46ed-9713-b1bb9e94f847" />

The application consists of several key components:

Frontend Hosting: AWS Amplify hosts our static web content
1. User Management: Amazon Cognito handles user registration and authentication
2. API Layer: Amazon API Gateway provides RESTful endpoints
3. Business Logic: AWS Lambda functions process ride requests
4. Data Storage: Amazon DynamoDB stores ride information
5. Version Control: GitHub integration for continuous deployment
   
🔗 Step 1: Connect GitHub & Deploy with Amplify

I connected my GitHub repo to AWS Amplify. With every push, Amplify automatically builds and deploys the latest version.

<img width="720" height="386" alt="image" src="https://github.com/user-attachments/assets/faf09b0a-7989-453b-867a-7c71d4b99be3" />
<img width="720" height="342" alt="image" src="https://github.com/user-attachments/assets/c90bec44-22de-4f44-ac8d-1f908bfdd2b9" />

🚀 Step 2: Watch It Deploy!
Amplify handles CI/CD like magic. Once I connected the repo, my app went live in minutes!

<img width="720" height="392" alt="image" src="https://github.com/user-attachments/assets/4b8e723f-3d32-446d-94b0-cb5ad5b53b46" />
<img width="720" height="386" alt="image" src="https://github.com/user-attachments/assets/01fa8a18-63dd-4e33-869f-6629ae0600e6" />

🔐 Step 3: Secure Users with Cognito
Security first! I integrated Amazon Cognito to let users sign up and sign in safely, with secure token management.

<img width="720" height="355" alt="image" src="https://github.com/user-attachments/assets/614b1431-cb86-46a8-ab04-128dc66c12b3" />
<img width="720" height="362" alt="image" src="https://github.com/user-attachments/assets/87813ad0-956a-4bf8-9404-d16aa1465073" />

update user pool Id and client Id on config.js

<img width="720" height="367" alt="image" src="https://github.com/user-attachments/assets/ea0e9850-4bf0-460f-a2f2-9cdb14a30b87" />

After updating config.js file

<img width="720" height="381" alt="image" src="https://github.com/user-attachments/assets/a118d82e-39bd-4fd5-ab81-76f090a52a82" />

you can login using your mail ID

<img width="720" height="404" alt="image" src="https://github.com/user-attachments/assets/f14771d2-4b8d-4a2c-a3b9-c1f337fd9e73" />
<img width="720" height="384" alt="image" src="https://github.com/user-attachments/assets/3430cdc6-740d-4651-913d-e316d02a35b8" />

🗂️ Step 4: Create the Database
I created a DynamoDB table to store all ride-sharing data, such as user ride requests, ride statuses, and driver details.

copy the database arn:

<img width="720" height="386" alt="image" src="https://github.com/user-attachments/assets/e15f4fd9-775d-4828-b6ee-ddba34031832" />

🛡️ Step 5: Create IAM Role (with Inline Policy)
I created an IAM role to allow my Lambda functions and API Gateway to securely access DynamoDB. To do this, I attached an inline policy with specific DynamoDB permissions. The inline policy uses the DynamoDB table ARN to restrict actions only to that table, following the principle of least privilege.

For example, I allowed actions like dynamodb:PutItem

<img width="720" height="388" alt="image" src="https://github.com/user-attachments/assets/c1bf0fbd-f0d5-4aea-9fe0-08c1d1dcfa53" />
<img width="720" height="373" alt="image" src="https://github.com/user-attachments/assets/59dc6d80-328e-4739-9e35-1e67e847c627" />
<img width="720" height="382" alt="image" src="https://github.com/user-attachments/assets/7f3c6fef-b2bb-4c58-9659-257132695023" />

⚙️ Step 6: Lambda Function & Backend Code

I created a Lambda function, attached the IAM role, and added this backend logic:

<img width="720" height="375" alt="image" src="https://github.com/user-attachments/assets/25916491-200d-4021-b2c5-bfe862749ddb" />

<img width="720" height="386" alt="image" src="https://github.com/user-attachments/assets/c5afac02-6437-4062-a189-e71500dff7db" />

{
“path”: “/ride”,
“httpMethod”: “POST”,
“headers”: {
“Accept”: “*/*”,
“Authorization”: “eyJraWQiOiJLTzRVMWZs”,
“content-type”: “application/json; charset=UTF-8”
},
“queryStringParameters”: null,
“pathParameters”: null,
“requestContext”: {
“authorizer”: {
“claims”: {
“cognito:username”: “the_username”
}
}
},
“body”: “{\”PickupLocation\”:{\”Latitude\”:47.6174755835663,\”Longitude\”:-122.28837066650185}}”
}

🌐 Example API Request
Example POST request to /ride through API Gateway:

{
“path”: “/ride”,
“httpMethod”: “POST”,
“headers”: {
“Accept”: “*/*”,
“Authorization”: “eyJraWQiOiJLTzRVMWZs”,
“content-type”: “application/json; charset=UTF-8”
},
“queryStringParameters”: null,
“pathParameters”: null,
“requestContext”: {
“authorizer”: {
“claims”: {
“cognito:username”: “the_username”
}
}
},
“body”: “{\”PickupLocation\”:{\”Latitude\”:47.6174755835663,\”Longitude\”:-122.28837066650185}}”
}

🌐 Step 7: API Gateway with Authorizer
I created an API Gateway REST API to expose my Lambda function. I added a Cognito Authorizer to secure the endpoints. The Authorizer validates the JWT token from Cognito before forwarding the request to Lambda.

In my config.js file on the frontend, I updated the API endpoint and authorizer configuration like this:

const config = {
api: {
invokeUrl: ‘https://your-api-id.execute-api.region.amazonaws.com/prod',
},
cognito: {
userPoolId: ‘YOUR_USER_POOL_ID’,
userPoolClientId: ‘YOUR_USER_POOL_CLIENT_ID’,
region: ‘YOUR_REGION’,
},
};

<img width="720" height="378" alt="image" src="https://github.com/user-attachments/assets/4a6b44cf-813c-45b7-821e-984af82b4b1b" />

<img width="720" height="368" alt="image" src="https://github.com/user-attachments/assets/bdee8396-49a7-418b-b391-391c6a4aff68" />

✅ Final Step: Fully Working Deployment
After deploying everything, I tested the full workflow:

* User signs up and logs in.
* The frontend calls the secured API Gateway.
* The Lambda function runs and saves ride data to DynamoDB.
* The live app is accessible via the Amplify URL.

<img width="720" height="385" alt="image" src="https://github.com/user-attachments/assets/74f1e834-9e72-48c2-bd77-ca24b2eeba02" />

<img width="720" height="381" alt="image" src="https://github.com/user-attachments/assets/1da555ab-763e-4682-b273-9203802b28a9" />

all the users information is stored in DynamoDB table

<img width="720" height="390" alt="image" src="https://github.com/user-attachments/assets/49bd1e32-5c72-44b7-9491-14e15db6e20c" />

🏁 Final Thoughts
This project shows how to build, test, and deploy a complete serverless app on AWS. It’s a great way to learn real-world cloud development!


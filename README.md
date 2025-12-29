# Serverless API Pattern
Serverless CRUD API - Lambda, API Gateway, DynamoDB

In this workshop, I attempted a Serverless API Pattern on AWS.

Architecture

<img width="795" height="359" alt="Screenshot 2025-12-19 at 4 27 55 PM" src="https://github.com/user-attachments/assets/aad0eb1a-752b-4b35-8e59-6fa471f950a9" />

Components
- Amazon API Gateway
- Acts as the entry point for clients over HTTPS.
- Exposes a single resource: /DynamoDBManager.
- Defines a POST method to handle requests.
- Translates incoming HTTP requests into Lambda invocations.

- AWS Lambda
- Interacts with DynamoDB to perform CRUD (Create, Read, Update, Delete) and Scan operations.

- Amazon DynamoDB
- NoSQL database that stores items manipulated by the Lambda function.
- Supports flexible operations such as item creation, updates, deletion, reads, and scans.

Steps

1 - IAM Role Creation 
- Create a Lambda Role in IAM.
- Named: Lambda2DynamoDB
- Attach permission from AWS Managed Policies
- AmazonDynamoDBFullAccess, CloudWatchLogsFullAccess

<img width="1249" height="647" alt="Screenshot 2025-12-19 at 5 38 01 PM" src="https://github.com/user-attachments/assets/08d7f3a7-2dc3-4b3f-8154-d244c5612aee" />

2 Create a Lambda function
- Name: CloudWatchLogsFullAccess
-LambdaFunctionOverHttps
- Tested the function
<img width="978" height="802" alt="Screenshot 2025-12-19 at 6 20 49 PM" src="https://github.com/user-attachments/assets/1ff0938b-05a2-4e82-8f2b-b5676f85ae4f" />

3. Create a DynamoDB table via AWS Console
- Table name: lambda-apigateway
- Partition key - id (string)

<img width="1539" height="708" alt="Screenshot 2025-12-19 at 6 23 41 PM" src="https://github.com/user-attachments/assets/4ee146c8-9a17-4e04-98f0-d0c630031679" />

4. Create API
- Use API Gateway Console
- Method - Post
<img width="1552" height="758" alt="Screenshot 2025-12-19 at 6 31 30 PM" src="https://github.com/user-attachments/assets/2543e362-35e0-444c-a0b5-98d4e6ee175c" />

5. Deploy API
- set Stage - New
- Stage - Prod

<img width="1375" height="595" alt="Screenshot 2025-12-19 at 6 39 51 PM" src="https://github.com/user-attachments/assets/a95638ab-1986-4336-ad92-e91bb691a3ea" />

6. Using CLI command: (this is so that AWS CLI knows which API to talk to)
I set the environment variables:
export API_ID=XXXXXX
export REGION=us-east-1
export STAGE=prod
export RESOURCE=dynamodbmanager

7. set $Body (This is to tell API what data to process)
```
export BODY='{
  "operation": "create",
  "tableName": "lambda-apigateway",
  "payload": {
    "Item": {
      "id": "1",
      "name": "Bob"
    }
  }
}'
```

9. Call test-invoke-method (using IAM Credential, executes Lambda)
aws --region $REGION --no-cli-pager apigateway test-invoke-method \
  --rest-api-id $API_ID \
  --resource-id <RESOURCE_ID_FROM_ABOVE> \
  --http-method POST \
  --body "$BODY"

10. Success Response

```
 Successfully completed execution\nMon Dec 29 15:35:16 UTC 2025 : Method completed with status: 200\n",
    "latency": 3188
```

10. I verified IAM Console to check results 
<img width="1661" height="743" alt="Screenshot 2025-12-29 at 10 56 59 AM" src="https://github.com/user-attachments/assets/81320f5a-851d-4708-bd00-7da9bc048036" />



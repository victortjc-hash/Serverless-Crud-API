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

- 

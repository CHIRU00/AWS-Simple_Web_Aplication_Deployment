# AWS-Simple_Web_Aplication_Deployment
This project applies skills from my Brainovision internship, using AWS services like Amplify, Lambda, API Gateway, and DynamoDB. I developed an end-to-end web application, incorporating serverless computing, API integration, and data storage. 
# AWS End-to-End Web Application Project

## Project Overview

This project aims to build an end-to-end web application using five AWS services. The application will:
1. Create and host a web page.
2. Invoke math functionality.
3. Store the result in a database.
4. Return the result to the user.
5. Handle permissions appropriately.

The following steps outline the entire process, including creating and hosting a web page using AWS Amplify, performing calculations with AWS Lambda, invoking the Lambda function via API Gateway, storing results in DynamoDB, and finally, deploying and testing the application.

### Prerequisites
- A text editor.
- An AWS account.
- Basic knowledge of AWS.

## Step-by-Step Guide

### 1. Create and Host a Web Page using AWS Amplify

#### HTML Code Example
Create a simple HTML page:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Math Application</title>
</head>
<body>
    <h1>Math Calculation</h1>
    <form id="mathForm">
        <input type="number" id="num1" placeholder="Enter first number">
        <input type="number" id="num2" placeholder="Enter second number">
        <button type="button" onclick="calculate()">Calculate</button>
    </form>
    <script>
        async function calculate() {
            const num1 = document.getElementById('num1').value;
            const num2 = document.getElementById('num2').value;
            const response = await fetch('YOUR_API_GATEWAY_URL', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ num1, num2 })
            });
            const result = await response.json();
            alert(`Result: ${result}`);
        }
    </script>
</body>
</html>
```

#### Deploying with Amplify
1. Go to AWS Amplify in the AWS Management Console.
2. Select "Host a web app."
3. Connect your repository or upload the HTML file.
4. Follow the instructions to deploy the app.

### 2. Use Lambda Function for Calculations

#### Create a Lambda Function
1. Navigate to AWS Lambda in the AWS Management Console.
2. Create a new function using Python.
3. Update the Lambda function code:
```python
import json
import math

def lambda_handler(event, context):
    num1 = event['num1']
    num2 = event['num2']
    result = num1 + num2  # or any other math operation
    return {
        'statusCode': 200,
        'body': json.dumps(result)
    }
```
4. Deploy the Lambda function.

### 3. Invoke Lambda Function with API Gateway

#### Create an API
1. Navigate to API Gateway in the AWS Management Console.
2. Create a new API and select REST API.
3. Create a new resource and method (POST).
4. Integrate the method with the Lambda function.
5. Enable CORS.
6. Deploy the API.

### 4. Store Results in DynamoDB

#### Setup DynamoDB
1. Navigate to DynamoDB in the AWS Management Console.
2. Create a new table with a primary key.
3. Note the Amazon Resource Name (ARN) for the table.

#### Update Lambda Function with DynamoDB Integration
1. Add necessary IAM permissions to the Lambda execution role.
2. Update the Lambda function code:
```python
import json
import math
import boto3
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('YourDynamoDBTableName')

def lambda_handler(event, context):
    num1 = event['num1']
    num2 = event['num2']
    result = num1 + num2  # or any other math operation
    timestamp = datetime.utcnow().isoformat()
    
    table.put_item(Item={
        'id': str(uuid.uuid4()),
        'result': result,
        'timestamp': timestamp
    })
    
    return {
        'statusCode': 200,
        'body': json.dumps(result)
    }
```
3. Deploy the updated Lambda function.

### 5. Test and Deploy the Application

#### Testing the Application
1. Open the deployed web page.
2. Enter numbers into the form and click "Calculate."
3. Verify the result is correct and displayed in an alert pop-up.

#### Redeploy with Updates
1. Update the `index.html` with any additional styling or functionality.
2. Zip the updated application files.
3. Redeploy using AWS Amplify.

### 6. Clean Up Resources
To avoid unnecessary costs, delete the following resources:
1. DynamoDB table.
2. Lambda function.
3. API Gateway.

## Conclusion
By following these steps, you have successfully built, deployed, and tested an end-to-end web application using AWS services. This project demonstrates how to integrate various AWS components to create a functional and efficient web application.

For further reference and detailed instructions, refer to the source code and documentation provided in this repository.

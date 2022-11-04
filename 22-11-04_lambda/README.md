# Exercise 03/11/22

Exercise to learn the Basics about lambda

### 1. Fetch Job data with a lambda function

1. create a s3 bucket for job data
1. write a lambda python function that fetch the job data from (https://www.arbeitnow.com/api/job-board-api) and creates a json file for every job inside of the s3 bucket
   (hint: to use requests you need to upload a source package or a layer with the dependency)

### 2. Integration

1. Fork the [cgn-aws-22-3-job-notifier](https://github.com/fabianschmauder/cgn-aws-22-3-job-notifier)
1. create a a new branch to add your lambda function
1. !!choose own bucket names for terraform state and src code!!
1. Add terraform and lambda function code (to create an s3 bucket you need a script)
1. create pull request

### 3. cloud watch trigger

1. add a cloud watch trigger that calls the lambda function every hour

### 4. CD (optional)

1. Setup a github actions pipeline that deploys your code and infrastructure ( you can use the cli to create the bucket)

<br/><br/>

# Solution by Irene

## Task 1.1

-   start lab, update credentials, download .pem and give permissions
-   in terminal:

    ```
    aws s3api create-bucket
                 --bucket ip-lamda-bucket
                 --region us-west-2
                 --create-bucket-configuration LocationConstraint=us-west-2
    ```

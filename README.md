# EDU demo Serverless + AWS Lambda + SQS Queue + Cron job

- Servererless
  - FOSS https://www.serverless.com/
  - for AWS: https://www.serverless.com/framework/docs/providers/aws/cli-reference/create/
  - manages multiple providers
  - install the CLI (`serverless` or alias `sls`)
- AWS Lambda
  <!-- - why? (img processing, Codeac.io example) -->
  - pricing https://aws.amazon.com/lambda/pricing/
  - register on AWS
  - download AWS CLI
  - log in to AWS CLI account first `aws configure`
  - generate serverless `serverless create --template aws-nodejs` (https://www.serverless.com/framework/docs/providers/aws/cli-reference/create/)
  - see the function and structrure
    - you can also set your own docker image instead of handler
  - set the function memory limit
    ```yaml
      memorySize: 254 # optional, in MB, default is 1024, minimum is 128
    ```
  - set environment and region
    ```yaml
      stage: demo
      region: eu-west-1
    ```
  <!-- - set billing alerts! -->
  <!-- - beware of public API endpoints! -->
  - set HTTP API endpoint <!-- and exaplain why, configures gateway -->
    ```yaml
      events:
        - httpApi:
          path: /hello
          method: get
    ```
  - deploy and see in AWS account `serverless deploy` or `sls deploy`
  - trigger API endpoint from the cmd output
  - see logs in console `sls logs -f hello`
  - see logs in CloudWatch <!-- https://eu-west-1.console.aws.amazon.com/cloudwatch/home?region=eu-west-1#logsV2:log-groups/log-group/$252Faws$252Flambda$252Fserverless-aws-lambda-sqs-demo-hello -->
  - see the function Code Source <!-- https://eu-west-1.console.aws.amazon.com/lambda/home?region=eu-west-1#/functions/serverless-aws-lambda-sqs-demo-hello?tab=code -->
- SQS (Simple Queue Service)
    <!-- - what is it? -->
    <!-- - pricing = free up to 1M reqs -->
    - set serverless SQS queue resource
      ```yaml
        resources:
          Resources:
            myQueue: # https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sqs-queues.html
              Type: "AWS::SQS::Queue"
              Properties:
                QueueName: "myQueue"
      ```
    - set the queue to the function
      ```yaml
        - sqs:
            arn:
              Fn::GetAtt:
                - myQueue
                - Arn
      ```
    - deploy and trigger queue + see logs
- Cron job
  <!-- - why? -->
  - set the schedule
    ```yaml
      - schedule: rate(1 minute)
    ```
- delete all with `serveless remove` or alias `sls remove`
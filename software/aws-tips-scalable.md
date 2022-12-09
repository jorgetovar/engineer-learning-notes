# AWS Tips for building scalable web applications 

1. Scale out and up. Use a machine whose properties and resources are a good fit for your workload (monitoring Cloudwatch metrics to do this). Make your application as redundant as possible, Multi-Region and Multi-Availability Zone 

2. Leverage Serverless services like S3, Lambda, SQS, Fargate, and SES

3. Cache information using API Gateway, Redis, and CDN

4. Make your components Stateless. Your application should be agnostic about which server handles it 5. Embrace asynchronous communication with SQS and SNS. DynamoDB streams are a great way to notify and handle DB events

6. Fault isolation, avoid cascade failure, and make your system reliable by implementing circuit breakers, timeouts, and bulkheads. etc

7. Make your system Observable, implement metrics, tracing, and good logging

8. Split your business logic by domain, compose your business logic with functions, and your application with components

9. Embrace immutability in your code (Avoid concurrent issues) and infrastructure (Terraform)

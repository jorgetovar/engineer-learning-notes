# Things to keep in mind when starting with AWS

Applications in the Cloud are reliable, and in some cases even cheaper than on-premises, but we all have made a lot of mistakes when started with AWS.

## Cost Optimization

First and one of the most important tips to keep in mind:

- Monitor your AWS costs by setting billing alerts & automate everything


**CloudFormation template**

```yaml
---
AWSTemplateFormatVersion: "2010-09-09"
Description: "Simple budget example"
Parameters:
  Email:
    Type: String
    Default: email@example.com
    Description: Please enter the email address to which budget notifications should be addressed.
Resources:
  BasicBudget:
    Type: "AWS::Budgets::Budget"
    Properties:
      Budget:
        BudgetLimit:
          Amount: 10
          Unit: USD
        TimeUnit: MONTHLY
        BudgetType: COST
      NotificationsWithSubscribers:
        - Notification:
            NotificationType: ACTUAL
            ComparisonOperator: GREATER_THAN
            Threshold: 99
          Subscribers:
            - SubscriptionType: EMAIL
              Address: !Ref Email
        - Notification:
            NotificationType: ACTUAL
            ComparisonOperator: GREATER_THAN
            Threshold: 80
          Subscribers:
          - SubscriptionType: EMAIL
            Address: !Ref Email
Outputs:
  BudgetId:
    Value: !Ref BasicBudget
```

- Tag everything 

## Performance Efficiency

- Use Serverless Architectures as much as possible
- Store no application state
- Log useful information and enable traceability

## Reliability

- Scale-out is better in most of the cases

## Security

- Create an IAM user for your AWS account.
- Enable MFA in all your AWS users
- Grant roles to EC2, applications should not have AIM accounts 
- Assign privileges to groups and not directly to the users


## Operational Excellence

- Leverage Infrastructure as Code: Terraform, CDK or even CloudFormation 
- Make things easy to rollback

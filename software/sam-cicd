
# Streamline AWS Development with CI/CD, SAM, and GitHub Actions

## Introduction:

When a good idea comes to mind, how can it be delivered to users as quickly as possible? for me, the solution is to implement CICD in combination with the combination of Testing and Infrastructure as Code.

**Continuous Integration and Continuous Deployment (CI/CD) has become essential practice in modern software development. 🚀**  

It enables teams to automate and streamline the process of building, testing, and deploying applications, reducing manual effort and ensuring faster and more reliable releases. In the AWS ecosystem, CI/CD can be seamlessly integrated with AWS Serverless Application Model (SAM) and GitHub Actions. 


![Pipeline](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5zfqt9j1p529ib6gq8to.png)


This article explores the power of CI/CD in AWS using SAM and GitHub Actions, along with an example of a ready-to-use Infrastructure as Code (IaC) template and integration testing as part of the pipeline.

[GitHub Repository](https://github.com/jorgetovar/sam-pipelines-cicd)

### Understanding CI/CD and its Benefits: 🚥

Continuous Integration is a practice where engineers integrate their work frequently, usually daily, the idea is that each integration is validated by a set of tests and automated builds.

Continuous Delivery is a practice where we build software in a way that it can be released to production at any time, usually daily and usually as one of the latest steps of a deployment pipeline.

Deploy software without manual intervention. You may implement manual approvals to deploy to production, but that's more of a business decision than a technical one.

We need to make frequent, automated releases of our software to reduce the feedback cycles and learn as much as possible from our users or customers.

The result is empowered teams and less stress in the process of releasing software.

**Done Means Released**

Finally, the idea is to improve continuously, learn and adapt.

### AWS Serverless Application Model (SAM): 🐿️

SAM is an open-source framework developed by AWS that simplifies the deployment and management of serverless applications. SAM extends AWS CloudFormation to provide a simplified syntax specifically designed for serverless resources.

```yaml
  CommunityBuilderFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: first_article/
      Handler: app.lambda_handler
      FunctionName: get-first-article
      Events:
        ArticleEvent:
          Type: Api
          Properties:
            Path: /v1/articles
            Method: post
            RestApiId: !Ref ApiDeployment
```
Benefits: 
- Simplified Serverless Application Development
- Local Development and Testing
- Deployment and Infrastructure as Code
- Built-in Best Practices
- Simplified CI/CD

### GitHub Actions: 🐙

GitHub Actions Is a CI/CD platform provided by GitHub that allows you to automate various workflows, tasks, and processes directly within your GitHub repository.

Secrets are important in the context of CI/CD servers because they allow you to save important information securely without exposing sensitive information.

### Building a CI/CD Pipeline with SAM and GitHub Actions: 🐙🐿️

We can define stages in the pipeline: build, unit test, integration test, and deployment.

```yaml
name: Pipeline

on:
  push:
    branches:
      - 'main'
...
  deploy-prod:
    if: github.ref == 'refs/heads/main'
    needs: [integration-test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: aws-actions/setup-sam@v2
        with:
          use-installer: true
      - uses: actions/download-artifact@v3
        with:
          name: packaged-prod.yaml

      - name: Assume the prod pipeline user role
        uses: aws-actions/configure-aws-credentials@v1-node16
        with:
          aws-access-key-id: ${{ env.PIPELINE_USER_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ env.PIPELINE_USER_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.PROD_REGION }}
          role-to-assume: ${{ env.PROD_PIPELINE_EXECUTION_ROLE }}
          role-session-name: prod-deployment
          role-duration-seconds: 3600
          role-skip-session-tagging: true

      - name: Deploy to production account
        run: |
          sam deploy --stack-name ${PROD_STACK_NAME} \
            --template packaged-prod.yaml \
            --capabilities CAPABILITY_IAM \
            --region ${PROD_REGION} \
            --s3-bucket ${PROD_ARTIFACTS_BUCKET} \
            --no-fail-on-empty-changeset \
            --role-arn ${PROD_CLOUDFORMATION_EXECUTION_ROLE}
```

### Infrastructure as Code (IaC) Template: 📖

Infrastructure as Code (IaC) is important because it enables consistent, reproducible, and automated provisioning and management of infrastructure, resulting in reduced manual errors in software development and deployment processes.

```yaml
Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function # More info about Function Resource: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction
    Properties:
      CodeUri: hello_world/
      Handler: app.lambda_handler
      Runtime: python3.9
      Architectures:
        - x86_64
      Events:
        HelloWorld:
          Type: Api # More info about API Event Source: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api
          Properties:
            Path: /hello
            Method: get
```
### Unit Testing and Integration Testing: 🦺

To ensure that we catch bugs and deliver reliable software, it is important to include automated tests in the build process. Test-driven development (TDD) is a useful tool in improving the design of the software and giving us the confidence to deploy it to production. The more comprehensive and well-written our tests are, the more confident we will be when deploying to production.

### Best Practices and Considerations: 🙌🏻

- Implementing security and compliance in CI/CD pipelines.
- Managing environment-specific configurations.
- Monitoring and logging for CI/CD pipelines.
- Handling rollbacks and canary deployments.

### Conclusion: 🤔

In this article, we have explored the powerful combination of CI/CD, AWS SAM, and GitHub Actions.

Finally, by leveraging CI/CD best practices, infrastructure as code, and automated testing, we can achieve faster, more reliable deployments while maintaining high-quality code.

I have always been a big fan of infrastructure as code, and I think Terraform, Pulumi, and CDK are also great options. However, if our idea is to deliver high-quality software, empower developers, and easily test a hypothesis in production, I would go with SAM. It's a powerful tool for developing serverless applications.

### Wrapping up
I would love to connect with you also on any of the following:

- [LinkedIn](https://www.linkedin.com/in/%F0%9F%91%A8%E2%80%8D%F0%9F%8F%AB-jorge-tovar-71847669/)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the posts please follow visit my blog [jorgetovar.dev](jorgetovar.dev)



# AWS Creating a Rest API with Infrastructure as Code (Terraform) & Serverless (Lambda + Python) - Part 2 CI/CD

In the previous post [Part 1](https://dev.to/aws-builders/creating-a-rest-api-with-infrastructure-as-code-terraform-serverless-lambda-python-part-1-3mbp), we saw how to deploy infrastructure using Terraform and quickly create an API with Python and Serverless. The example provided us with the fundamentals of key components and services for infrastructure creation. In this article, the idea is to automate the deployment using a platform that has always seemed excellent to me, like CircleCI. We will also see how to manage the state of resources from Terraform Cloud and, finally, we will list best practices to consider when creating an API.

[GitHub Repository](https://github.com/jorgetovar/serverless-terraform)

## Continuous Integration and Continuous Deployment

Continuous Integration and Continuous Deployment (CI/CD) are essential practices in modern software development. They enable teams to automate and optimize the process of building, testing, and deploying applications, reducing manual effort and ensuring faster and more reliable releases.


![Terraform Apply](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l4v8ryhd18b1zoedutc6.png)


### CircleCI

CircleCI is a CI/CD platform that automates and streamlines the process of building, testing, and deploying applications. It provides a scalable and flexible infrastructure for running CI/CD workflows in the cloud. Configuring and using CircleCI is simpler compared to setting up Jenkins and its associated infrastructure. Additionally, CircleCI offers core features for free and also includes the option to set up manual approvals, which are paid in other platforms like GitHub Actions.


```yaml
- plan-apply:
    filters:
     branches:
      only:
        - main
- hold-apply:
    type: approval
    requires:
      - plan-apply
- apply:
    requires:
      - hold-apply
```
Implementation of manual approval, one of the most important and easy-to-implement features.

![CircleCI](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/08dxzj0f9fax0xm29lje.png)


## Lambda Layers

Lambda Layers allow separating and reusing common code components such as libraries, dependencies, or static files. This facilitates the development, maintenance, and updating of Lambda functions by providing an efficient way to manage and share shared resources among multiple functions. Lambda Layers also enable integration with powerful libraries like Powertools to implement observability with tracing, logging, and metrics.

The code should be placed inside the Python folder.


![Layers](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ax6lkqwl8ltv9494r6h9.png)

For example, if we want to use the requests library, we could either install the entire package in the same folder or, even better, put the layer with the shared code.


![Terraform Layers](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h4ha3qza6any0848u70r.png)


## Best Practices

1. Do not keep branches for a long time in the software repository. It is recommended to integrate with the main branch every day.
2. Version all code and infrastructure. Proper version control helps with tracking and managing changes.
3. Perform regular unit and integration tests to release software to production with confidence.
4. Implement appropriate security measures, such as authentication and authorization, to protect your API from unauthorized access.
5. Use monitoring tools like the ELK Stack to collect and analyze real-time performance data.
6. Generate interactive documentation using tools like Swagger or Postman to facilitate the consumption of your API.
7. Minimize privileges and securely store credentials used to deploy the infrastructure

.

### Security

In Amazon API Gateway, you can configure rate limit policies to restrict the number of requests per time interval and prevent abuse of the API. Additionally, you can enable authentication options in API Gateway, such as API key-based authentication, authorization tokens, or integration with identity providers like AWS Cognito or custom IAM services, to ensure that only authorized users can access the API and its protected resources. These combined rate limit and authentication measures provide security and control over API access and usage.

Currently, the call can be made without any validation, simply by knowing the endpoint.

`http "$(terraform output -raw base_url)/hello"`

### Remote State

In the previous post, we left out Terraform state files that remain on the local machine. Ideally, all changes should be executed from a server, controlling granularity and keeping a record of all changes. Therefore, we will use Terraform Cloud to configure and manage everything securely from another environment, without dealing with state files.

Terraform Cloud offers benefits such as centralized management of infrastructure state, remote execution of plans and applications, and change tracking and notifications to keep a clear track of modifications to the infrastructure.


![Resources](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w2r67ibpnpgdnew3w5sv.png)


## Conclusion

Automation, software-as-a-service (SaaS) platforms, artificial intelligence, and other technologies are increasingly facilitating the generation of applications within a framework of best practices. However, the development process remains a human matter, where mindset and professionalism allow us to reach the next level of excellence.

It is important to emphasize that the biggest problems in the software development and delivery process are organizational and people-related. In the end, building applications is a social activity, but the more we solve technical problems and improve communication, the easier it should be to reach production. Happy coding! 🎉

- [LinkedIn](https://www.linkedin.com/in/%F0%9F%91%A8%E2%80%8D%F0%9F%8F%AB-jorge-tovar-71847669/)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog [jorgetovar.dev](jorgetovar.dev)
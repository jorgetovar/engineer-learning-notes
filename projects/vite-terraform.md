# vite-aws-terraform

**"The slow feedback loop can greatly affect developers' productivity and happiness."**

http://vite-aws-website-bucket.s3-website-us-east-1.amazonaws.com/


A couple of days ago, I read about Vite: a powerful and fast tool. I created a demo to see it in action. Although a very basic one, it seems like Vite is gaining traction for his outstanding performance and compilation times.

This repository is a getting started example about how to create a static website simply with IaC, compilation time of milliseconds, and CI/CD. The tech stack includes:

- [Vite](https://vitejs.dev/): Next Generation Frontend Tooling 
(https://github.com/jorgetovar/vite-aws-terraform/tree/main/vite-aws-terraform-app)

- [AWS](https://aws.amazon.com/): Hosting the website using S3 (In the future I will post an update with Cloudfront and Route53)

- [Terraform](https://www.terraform.io/): Infrastructure as Code 
(https://github.com/jorgetovar/vite-aws-terraform/tree/main/infra)

- [CircleCI](https://circleci.com/): Continuous integration and deployment
(https://github.com/jorgetovar/vite-aws-terraform/tree/main/.circleci)


## Vite (Ultrafast hot-reload and build)

It can often take an unreasonably long wait to spin up a dev server. Component updates can take a couple of seconds or even minutes in some cases to be reflected in the browser. Vite aims to address these issues.

```javascript
npm create vite@latest
npm run dev
npm run build
```

## AWS
Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. 


## Terraform

Terraform is an open-source infrastructure as code software tool that provides a consistent CLI workflow to manage hundreds of cloud services. 

1. Create the remote backend to handle the terraform state (Information about what resources have been created)
2. Create the bucket and apply the policies and rules needed
 
```shell
terraform init
terraform plan
terraform apply
```

```shell

➜  backend-state git:(initial-commit) ✗ terraform apply
var.state_bucket_name
  The name of the S3 bucket. Must be globally unique.

  Enter a value: vite-aws-terraform


Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:
...


aws_dynamodb_table.terraform_locks: Creating...
aws_s3_bucket.terraform_state: Creating...
aws_s3_bucket.terraform_state: Creation complete after 8s [id=vite-aws-terraform]
aws_s3_bucket_public_access_block.terraform_state_policy: Creating...
aws_s3_bucket_public_access_block.terraform_state_policy: Creation complete after 1s [id=vite-aws-terraform]
aws_dynamodb_table.terraform_locks: Still creating... [10s elapsed]
aws_dynamodb_table.terraform_locks: Creation complete after 11s [id=vite-aws-terraform-locks]

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

Outputs:

dynamodb_table_name = "vite-aws-terraform-locks"
s3_bucket_arn = "arn:aws:s3:::vite-aws-terraform"
 ```

## CircleCI

Fast, customizable, and reliable service to create pipelines and automate your deployments. The ORBs make it very easy to integrate and deploy in AWS and others providers. 

**12 seconds to update the website.**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tl64lm7496fuvgs1qriu.png)

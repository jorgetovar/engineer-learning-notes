# zero-downtime-deployment-tofu

I still remember the days when we had to set flags and display a maintenance page during application deployments. Often, we deployed our applications at midnight or during periods of low traffic to minimize the impact on users, which led to a poor developer experience. **Nowadays, as clients, we expect a seamless user experience at all times**.


## Why Zero-Downtime Deployment

Achieving zero-downtime deployments has become a requirement for modern applications. We are now in an era of software delivery where downtime should be minimal and latency low. **As clients, our expectations have raisen.** If an application does not function correctly, we perceive it as unreliable and are likely to abandon its use. 🤑

Today, I want to introduce some strategies to implement zero downtime deployments and build trust with your customers.

[GitHub Code](https://github.com/jorgetovar/zero-downtime-deployment-tofu)

- Use an instance refresh to update instances in an Auto Scaling group (OpenTofu)
- Immutable infrastructure + Mutable Applications (Terraform + Ansible)
- Implement Blue/Green deployments
- Canary Deployments (Deploying Serverless Applications Gradually)

We aim to avoid creating unreliable applications and instead embrace a cloud-native approach.

![Downtime](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1rozg2ks7655lvo8g154.jpeg)

Let’s delve into the strategies:

## Use an Instance Refresh to Update Instances in an Auto Scaling Group (OpenTofu)

For this example, we are using [OpenTofu](https://opentofu.org/). I want to emphasize that open-source software is incredibly beneficial—the quality is exceptionally high, and it allows us to leverage numerous resources and build upon code developed by excellent engineers. We can learn significantly from these kinds of projects. That is also one of the reasons that the code here is a synthesis of many ideas and is available to anyone who wants to learn and reuse it.

> [The Future of Terraform Must Be Open](https://medium.com/gruntwork/the-future-of-terraform-must-be-open-ab0b9ba65bca)

![ASG](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fbyx7kpbarhhtkwt9bhq.png)

We need to define three important attributes: `min_elb_capacity`, `lifecycle`, and `instance_refresh`. These attributes will serve the following purposes:


- **min_elb_capacity**: This attribute ensures that the deployment is considered complete only after at least some instances have passed health checks.

- **lifecycle**: This attribute helps prevent the deletion of previous resources after the new ones have been created.

- **instance_refresh**: This attribute defines a strategy for rolling out changes to the Auto Scaling Group (ASG).


```terraform
resource "aws_autoscaling_group" "zdd" {

  name                = "${var.cluster_name}-${aws_launch_template.zdd.name}"
  vpc_zone_identifier = var.subnets
  target_group_arns = [aws_lb_target_group.asg.arn]
  health_check_type   = "ELB"

  min_elb_capacity = var.min_size

  min_size = var.min_size
  max_size = var.max_size

  lifecycle {
    create_before_destroy = true
  }

  # Use instance refresh to roll out changes to the ASG
  instance_refresh {
    strategy = "Rolling"
    preferences {
      min_healthy_percentage = 50
    }
  }

  launch_template {
    id      = aws_launch_template.zdd.id
    version = "$Latest"
  }

  tag {
    key                 = "Name"
    value               = var.cluster_name
    propagate_at_launch = true
  }

}

```

The result is an API that returns data reliably without becoming unavailable.

![Deployment example](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3vq1ii8qefcbndjtwmrt.png)

In this example, we rely on immutable infrastructure, so we need to pack and create a new AMI whenever there is a change.

```sh
packer build sample-app.pkr.hcl
tofu apply 
```

## Immutable infrastructure + Mutable Applications (Terraform + Ansible)

Even though we can do everything with provisioning tools such as Terraform or CloudFormation, sometimes it is better to use them together with configuration management tools like Ansible. Ansible is designed to manage and install software on servers, providing a useful approach for handling zero-downtime deployments.

![Ansible](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ks8u3tmqfwu5w29e7a3i.png)

We can serve our application with Nginx

```yaml
  - name: Install Nginx
    hosts: all
    become: true

    tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Add index page
      template:
        src: index.html
        dest: /var/www/html/index.html

    - name: Start Nginx
      service:
        name: nginx
        state: started

```
The result is an application that can be easily updated with an Ansible command, leading to zero downtime.

![Ansible update](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pirvnssbekf58da11nr8.png)


## Blue/Green Deployments

The idea behind this approach is to have two environments running different versions of your application. You can gradually roll out a percentage of the changes or do it all at once. The blue environment serves traffic from users in production, while the green environment hosts the new version of the application where you can perform tests and validations. After the green environment is tested, you can redirect traffic to it. If you encounter any problems, you can roll back by redirecting traffic to the blue environment.

![Blue-Green](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pkyottr4ri47h4dl7h4u.png)

One of the most amazing things about AWS and Terraform is their community. As you can see, we can leverage public modules to compose our application. In this case, we switch between environments using a flag. Each time we update our application, the new version remains idle and available, while the former live server continues receiving traffic until we are confident that the idle server is functioning properly and make the switch.


```terraform
provider "aws" {
  region  = "us-east-2"
}

variable "production" {
  default = "green"
}

module "base" {
  source     = "terraform-in-action/aws/bluegreen//modules/base"
  production = var.production
}

module "green" {
  source      = "terraform-in-action/aws/bluegreen//modules/autoscaling"
  app_version = "v1.0"
  label       = "green"
  base        = module.base
}

module "blue" {
  source      = "terraform-in-action/aws/bluegreen//modules/autoscaling"
  app_version = "v2.0"
  label       = "blue"
  base        = module.base
}

output "lb_dns_name" {
  value = module.base.lb_dns_name
}

```

## Canary Deployments (Deploying Serverless Applications Gradually)

This strategy involves gradually releasing the software to a small subset of users. By doing so, we can validate whether everything is working as expected and continue to increase the percentage of users who can use the newest version of the application. If a problem is identified, we can easily roll back by reducing the amount of traffic to the new version to zero.

With SAM, it is possible to perform traffic validations and rollbacks out of the box using CloudWatch alarms. This is very handy when changes in the logic might be risky.

![CodeDeploy](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/29kw8pwixyfs4c7jl95l.png)

Canary deployments are more focused on people than servers. In this case, we focus on the number of users who will experience the new version of the Lambda functions. We can monitor our application and automatically roll back if needed. Keep in mind that this process is safer but slower. Here are the options available in AWS SAM:


- *Canary10Percent<X>Minutes*
- *Linear10PercentEvery<X>Minutes*
- *AllAtOnce*


```yaml
Resources:
  AWSCommunityLambda:
    Type: AWS::Serverless::Function
    Properties:
      Handler: index.py
      Runtime: python3.11
      CodeUri: functions/api/handler
      AutoPublishAlias: live
      DeploymentPreference:
        Type: Canary10Percent10Minutes 
        Alarms:
          - !Ref AliasErrorMetricGreaterThanZeroAlarm
          - !Ref LatestVersionErrorMetricGreaterThanZeroAlarm
        Hooks:
          PostTraffic: !Ref PostTrafficLambdaFunction
```

## Conclusions

- Zero-downtime deployments are key to building trust in the highly dynamic world of cloud-native applications.
- SAM has powerful options to deploy infrastructure in a safe manner.
- Ansible can work in combination with Terraform to create a better DevOps experience by reducing the blast radius through just updating the application.
- Immutable infrastructure is usually a better approach because you have the same configuration deployed consistently, rather than a server where many changes have been applied, potentially leading to configuration differences.
- Immutable infrastructure also has downsides, such as requiring the rebuilding of an image from a server template and redeploying for a simple change.
- OpenTofu is an excellent option for deploying infrastructure. I haven't tried it before, but it seems like the community is doing a great job, and the new releases will have more and more features that are not yet supported by Terraform.


## References

1. [Terraform in Action](https://learning.oreilly.com/library/view/terraform-in-action/9781617296895/OEBPS/Text/Ch-09.htm#sigil_toc_id_148) - Practical uses and strategies for implementing Terraform, a tool for building, changing, and managing infrastructure.
2. [Terraform: Up & Running](https://learning.oreilly.com/library/view/terraform-up-and/9781098116736/ch05.html#idm46165904409072) - Friendly introduction to Terraform, including its complex concepts, a lot of examples and how to get started with it.
3. [GruntWork](https://www.gruntwork.io/fundamentals-of-devops)

Not too long ago, deploying in production was a tedious task that involved multiple actors. Nowadays, we have a wealth of open-source software and developer-focused tools that emphasize immutability, automation, productivity-enhancing AI, and powerful IDEs. 

**There has never been a better time to be an engineer and create value in society through software.**

- [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog [jorgetovar.dev](jorgetovar.dev)

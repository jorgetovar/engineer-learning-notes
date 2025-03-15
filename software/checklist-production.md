# AWS Deployment Checklist

A comprehensive checklist for deploying applications on AWS, covering server-side deployment, client-side optimization, data storage, scalability, continuous integration, continuous delivery, networking, security, monitoring, and cost optimization.

## Table of Contents
- [Server-side](#server-side)
- [Client-side](#client-side)
- [Data Storage](#data-storage)
- [Scalability and High Availability](#scalability-and-high-availability)
- [Continuous Integration](#continuous-integration)
- [Continuous Delivery](#continuous-delivery)
- [Networking](#networking)
- [Security](#security)
- [Monitoring](#monitoring)
- [Cost Optimization](#cost-optimization)

## Server-side

- [ ] **Build AMIs**  
  Package stateful applications as [Amazon Machine Images (AMIs)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) using tools like [Packer](https://www.packer.io/).

- [ ] **Deploy AMIs using Auto Scaling Groups**  
  Run AMIs in [Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) to manage instance lifecycle and scaling.

- [ ] **Build Docker images**  
  Package stateless apps as [Docker](https://docker.com/) images and push to [Elastic Container Registry (ECR)](https://aws.amazon.com/ecr/).

- [ ] **Deploy Docker images using ECS, EKS, or Fargate**  
  Run Docker containers using [Elastic Container Service (ECS)](https://aws.amazon.com/ecs/), [Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/), or [AWS Fargate](https://aws.amazon.com/fargate/).

- [ ] **Deploy serverless apps using Lambda and API Gateway**  
  Build serverless applications with [AWS Lambda](https://aws.amazon.com/lambda/) and [API Gateway](https://aws.amazon.com/api-gateway/).

- [ ] **Configure CPU, memory, and GC settings**  
  Optimize application resource settings based on [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/).

- [ ] **Configure hard drives**  
  Set up [root volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html) and attach [EBS Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumes.html) for persistent storage.

## Client-side

- [ ] **Pick a JavaScript framework**  
  Consider [React](https://reactjs.org/), [Angular](https://angular.io/), or [Ember](https://www.emberjs.com/).

- [ ] **Pick a compile-to-JS language**  
  Consider [TypeScript](https://www.typescriptlang.org/), [Scala.js](https://www.scala-js.org/), [PureScript](https://github.com/paf31/purescript), [Elm](https://elm-lang.org/), or [ClojureScript](https://github.com/clojure/clojurescript).

- [ ] **Pick a compile-to-CSS language**  
  Consider [SASS](https://sass-lang.com/), [less](https://lesscss.org/), [cssnext](https://cssnext.io/), or [postcss](https://github.com/postcss/postcss).

- [ ] **Optimize your assets**  
  Minify CSS/JS, compress images, and use tools like [Grunt](https://gruntjs.com/), [Gulp](https://gulpjs.com/), [Broccoli](https://github.com/broccolijs/broccoli), or [webpack](https://webpack.js.org/).

- [ ] **Use a static content server**  
  Serve static content from [S3](https://aws.amazon.com/s3/).

- [ ] **Use a CDN**  
  Implement [CloudFront](https://aws.amazon.com/cloudfront/) for content distribution.

- [ ] **Configure caching**  
  Implement versioning, caching, and cache-busting strategies for static content.

## Data Storage

- [ ] **Deploy relational databases**  
  Use [RDS](https://aws.amazon.com/rds/) or [Amazon Aurora](https://aws.amazon.com/rds/aurora/).

- [ ] **Deploy NoSQL databases**  
  Consider [Elasticache](https://aws.amazon.com/elasticache/) for Redis/Memcached, [DynamoDB](https://aws.amazon.com/dynamodb/) for key-value storage, or [DocumentDB](https://aws.amazon.com/documentdb/) for MongoDB compatibility.

- [ ] **Deploy queues**  
  Implement [SQS](https://aws.amazon.com/sqs/) for distributed queuing.

- [ ] **Deploy search tools**  
  Use [Amazon Elasticsearch](https://aws.amazon.com/elasticsearch-service/) or [CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html).

- [ ] **Deploy stream processing tools**  
  Consider [Amazon MSK](https://aws.amazon.com/msk/) (Managed Kafka) or [Kinesis](https://aws.amazon.com/kinesis/).

- [ ] **Deploy a data warehouse**  
  Use [Amazon Redshift](https://aws.amazon.com/redshift/) for data warehousing.

- [ ] **Deploy big data systems**  
  Implement [Amazon EMR](https://aws.amazon.com/emr/) for Hadoop, Spark, etc.

- [ ] **Set up cron jobs**  
  Use [Lambda Scheduled Events](https://docs.aws.amazon.com/lambda/latest/dg/with-scheduled-events.html) or [ECS Scheduled Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/scheduled_tasks.html).

- [ ] **Configure disk space**  
  Plan storage with appropriate [EBS Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumes.html).

- [ ] **Configure backup**  
  Set up automated backups for all data stores.

- [ ] **Configure cross-account backup**  
  Copy backups to a separate AWS account for redundancy.

- [ ] **Test your backups**  
  Regularly restore from backups to ensure they work.

- [ ] **Set up schema management**  
  Use tools like [Flyway](https://flywaydb.org/) or [Liquibase](https://www.liquibase.org/) for database migrations.

## Scalability and High Availability

- [ ] **Choose between a Monolith and Microservices**  
  Consider whether a monolithic architecture might be simpler initially.

- [ ] **Configure service discovery**  
  Set up [Load Balancers](https://aws.amazon.com/elasticloadbalancing/), [ECS Service Discovery](https://aws.amazon.com/blogs/aws/amazon-ecs-service-discovery/), or [Consul](https://www.consul.io/).

- [ ] **Use multiple Instances**  
  Run more than one copy of each stateless application.

- [ ] **Use multiple Availability Zones**  
  Configure services across multiple [Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html).

- [ ] **Set up load balancing**  
  Use [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) for HTTP/HTTPS and [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) for other protocols.

- [ ] **Use Auto Scaling**  
  Implement [auto scaling](https://aws.amazon.com/autoscaling/) for dynamic resource management.

- [ ] **Configure Auto Recovery**  
  Use process supervisors and health checks for automatic recovery.

- [ ] **Configure graceful degradation**  
  Implement retries, circuit breaking, timeouts, and rate limiting.

- [ ] **Perform load tests and use chaos engineering**  
  Test infrastructure resilience with load testing and [chaos engineering](https://principlesofchaos.org/).

## Continuous Integration

- [ ] **Pick a Version Control System**  
  Set up [Git](https://git-scm.com/) with [GitHub](https://github.com/), [GitLab](https://gitlab.com/), or [BitBucket](https://bitbucket.org/).

- [ ] **Do code reviews**  
  Establish a [pull request](https://help.github.com/articles/about-pull-requests/) process.

- [ ] **Configure a build system**  
  Implement tools like [Gradle](https://gradle.org/), [Rake](https://github.com/ruby/rake), or [Yarn](https://yarnpkg.com/en/).

- [ ] **Use dependency management**  
  Explicitly define and lock dependencies.

- [ ] **Configure static analysis**  
  Set up [static analysis tools](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis) like linters.

- [ ] **Set up automatic code formatting**  
  Configure tools like `go fmt` or `terraform fmt`.

- [ ] **Set up automated tests**  
  Implement testing with [JUnit](https://junit.org/junit5/), [RSpec](https://rspec.info/), or [Mocha](https://mochajs.org/).

- [ ] **Publish versioned artifacts**  
  Create immutable, versioned deployment artifacts.

- [ ] **Set up a build server**  
  Configure [CircleCI](https://circleci.com/), [Travis CI](https://travis-ci.org/), or [Jenkins](https://jenkins.io/).

## Continuous Delivery

- [ ] **Create deployment environments**  
  Set up separate dev, stage, and prod environments.

- [ ] **Set up per-environment configuration**  
  Define environment-specific configs in version control.

- [ ] **Define your infrastructure as code**  
  Use [Terraform](https://www.terraform.io/), [Packer](https://www.packer.io/), and [Docker](https://www.docker.com/).

- [ ] **Test your infrastructure code**  
  Implement tests with tools like [Terratest](https://github.com/gruntwork-io/terratest).

- [ ] **Set up immutable infrastructure**  
  Deploy new instances rather than modifying existing ones.

- [ ] **Promote artifacts**  
  Deploy the same immutable artifacts across environments.

- [ ] **Roll back in case of failure**  
  Have procedures to revert to previous versions.

- [ ] **Automate your deployments**  
  Create fully automated deployment pipelines.

- [ ] **Do zero-downtime deployments**  
  Implement [blue-green deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html) or [rolling deployment](https://hintcafe.net/post/56948449558/rolling-deployment-with-no-downtime).

- [ ] **Use canary deployments**  
  Deploy to a subset of servers first to detect issues.

- [ ] **Use feature toggles**  
  Implement [feature toggles](https://martinfowler.com/articles/feature-toggles.html) to separate deployment from release.

## Networking

- [ ] **Set up VPCs**  
  Create custom [Virtual Private Clouds](https://aws.amazon.com/vpc/) with appropriate [sizing](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#VPC_Sizing).

- [ ] **Set up subnets**  
  Create public, private-app, and private-persistence [subnets](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html).

- [ ] **Configure Network ACLs**  
  Set up [Network Access Control Lists](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html) for subnet-level security.

- [ ] **Configure Security Groups**  
  Create [Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) following least privilege principles.

- [ ] **Configure Static IPs**  
  Use [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) or [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html).

- [ ] **Configure DNS using Route 53**  
  Manage DNS with [Route 53](https://aws.amazon.com/route53/) and consider [Private Hosted Zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-private.html).

## Security

- [ ] **Configure encryption in transit**  
  Implement TLS for all network connections and use [AWS Certificate Manager (ACM)](https://aws.amazon.com/certificate-manager/) for free, auto-renewing certificates.

- [ ] **Configure encryption at rest**  
  Enable encryption for [EBS Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) and other services.

- [ ] **Set up SSH access**  
  Configure individual SSH access rather than shared keys.

- [ ] **Deploy a Bastion Host**  
  Set up a secure entry point to your network.

- [ ] **Deploy a VPN Server**  
  Consider [OpenVPN](https://openvpn.net/) for secure network access.

- [ ] **Set up a secrets management solution**  
  Use [KMS](https://aws.amazon.com/kms/), [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/), [SSM Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html), or [HashiCorp Vault](https://www.vaultproject.io/).

- [ ] **Use server hardening practices**  
  Implement [CIS Hardened Images](https://www.cisecurity.org/services/hardened-virtual-images/), [fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page), and automatic security updates.

- [ ] **Go through the OWASP Top 10**  
  Check for [OWASP Top 10](https://www.owasp.org/index.php/Top_10-2017_Top_10) vulnerabilities.

- [ ] **Go through a security audit**  
  Conduct third-party security audits and penetration testing.

- [ ] **Sign up for security advisories**  
  Monitor security announcements for used software.

- [ ] **Create IAM Users**  
  Set up individual [IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) for each developer.

- [ ] **Create IAM Groups**  
  Manage permissions with [IAM Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html).

- [ ] **Create IAM Roles**  
  Use [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) for AWS resource access.

- [ ] **Create cross-account IAM Roles**  
  Configure [cross-account roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) for multi-account setups.

- [ ] **Create a password policy and enforce MFA**  
  Set a strong [password policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html) and require [MFA](https://aws.amazon.com/iam/details/mfa/).

## Monitoring

- [ ] **Track availability metrics**  
  Use [Route 53 Health Checks](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html) and [Pingdom](https://www.pingdom.com/) to monitor service availability.

- [ ] **Track business metrics**  
  Monitor user activity with [Google Analytics](https://www.google.com/analytics), [Kissmetrics](https://www.kissmetrics.com/), or [Mixpanel](https://mixpanel.com/).

- [ ] **Track application metrics**  
  Monitor QPS, latency, and throughput using [CloudWatch](https://aws.amazon.com/cloudwatch/), [DataDog](https://www.datadoghq.com/), or [New Relic](https://newrelic.com/).

- [ ] **Track server metrics**  
  Monitor CPU, memory, and disk usage with [CloudWatch](https://aws.amazon.com/cloudwatch/), [Nagios](https://www.nagios.org/), or [collectd](https://collectd.org/).

- [ ] **Configure services for observability**  
  Use [Kafka](https://kafka.apache.org/), [Honeycomb](https://honeycomb.io/), and [OpenTracing](https://opentracing.io/) for event tracking and analysis.

- [ ] **Store logs**  
  Set up log rotation and aggregation using [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), [Filebeat](https://www.elastic.co/products/beats/filebeat), or [Papertrail](https://papertrailapp.com/).

- [ ] **Set up alerts**  
  Configure alerts for critical metrics and use [PagerDuty](https://www.pagerduty.com/) or [VictorOps](https://victorops.com/) for on-call rotations.

## Cost Optimization

- [ ] **Pick proper EC2 Instance types and sizes**  
  Use [EC2Instances.info](https://www.ec2instances.info/) to compare instance types and optimize for performance vs cost.

- [ ] **Use Spot EC2 Instances for background jobs**  
  Leverage [EC2 Spot Instances](https://aws.amazon.com/ec2/spot/) for non-urgent workloads to save up to 90% on costs.

- [ ] **Use Reserved EC2 Instances for dedicated work**  
  Purchase [EC2 Reserved Instances](https://aws.amazon.com/ec2/pricing/reserved-instances/) for consistent workloads to save up to 75%.

- [ ] **Shut down EC2 Instances and RDS DBs when not using them**  
  Use [AWS Instance Scheduler](https://aws.amazon.com/answers/infrastructure-management/instance-scheduler/) to automatically shut down resources during off-hours.

- [ ] **Use Auto Scaling**  
  Implement [Auto Scaling](https://aws.amazon.com/autoscaling/) to optimize instance count based on load.

- [ ] **Use Docker when possible**  
  Deploy with [ECS](https://aws.amazon.com/ecs/) to efficiently utilize resources across multiple applications.

- [ ] **Use Lambda when possible**  
  Utilize [AWS Lambda](https://aws.amazon.com/lambda/) for short-running tasks to minimize infrastructure costs.

- [ ] **Clean up old data with S3 Lifecycle settings**  
  Configure [S3 Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) to optimize storage costs.

- [ ] **Clean up unused resources**  
  Use tools like [cloud-nuke](https://github.com/gruntwork-io/cloud-nuke) to remove unused AWS resources.

- [ ] **Learn to analyze your AWS bill**  
  Use [Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) and [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/) to understand and optimize costs.

- [ ] **Create billing alarms**  
  Set up [billing alerts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html) to monitor and control AWS spending.
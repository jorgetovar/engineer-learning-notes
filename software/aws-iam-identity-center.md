# How to set up Single Sign-On in AWS (IAM Identity Center)

When I was at university, I met some programmers who were finishing their software engineering degrees. Some of them were already working, and they gave me what is still one of the best pieces of advice I’ve ever received about this craft:

> *“If you want to be good, stick with university knowledge. If you want to be great, become a lifelong learner.”*

When I started learning AWS, **having a personal account was the single best investment I made**. It felt risky at first—you need to add your credit card, and you might accidentally leave an EC2 instance running or overspend on a machine. (That’s why budgets, policies, and guardrails are so important—but that’s a topic for another post.)

Once I got past that initial fear, I realized it was a great time to start using multiple accounts. Not only could I access more free credits, but I also unlocked the opportunity to practice real-world multi-account use cases, such as:

* Shared Data Lake
* RDS Cross-Account Database Access
* VPC Peering
* Centralized Logging

At first, I managed my AWS accounts with just some personal notes. But as the number of accounts grew, it became harder to keep track, and I found myself juggling multiple browsers just to run experiments. Eventually, I moved to a better strategy: using **AWS Organizations** and, later, **IAM Identity Center** to enable Single Sign-On across all my accounts.

That’s exactly what you’re going to learn in this post. Hopefully, this will unlock tons of learning opportunities for you too. Having multiple accounts gives you the freedom to:

* Launch pet projects without risking production
* Monitor and experiment in isolation
* Explore advanced AWS use cases

And most importantly: it helps you **avoid doing experiments in your company’s account**. Even though it might seem cheaper, it’s far riskier—I’ve heard plenty of horror stories about that.

The first requirement to implement AWS SSO is having a valid AWS Organization.

---

## AWS Organizations

AWS Organizations is a service that helps you centrally manage and govern multiple AWS accounts, with benefits such as:

* Consolidated Billing
* Service Control Policies (SCPs)
* AWS SSO Integration (one of the main advantages of having organizations in place)

---

## AWS SSO

AWS Single Sign-On (SSO) enables you to centrally manage SSO access and user permissions across your AWS accounts. Typically, when you log into an AWS account, you use IAM users and must authenticate manually for each account. The first problem with this approach is that you sometimes need to provide AWS credentials to the same person across different accounts, which quickly becomes complex to manage.

---

## Demo

### 1. Enable SSO

![IAM identity center](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gd2dxtb0vl3205035nnr.png)

* Sign in to the AWS Identity and Access Management console.
* Open the Identity Center.
* Choose **Enable**.
* Choose **Enable with AWS Organizations**.
* Choose **Continue**.

After that, you can customize the *AWS access portal URL*. Stick to conventions and remember it has to be unique.

---

### 2. Permission Sets

![Permissions set](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y9v8nmxvhm28mi1a0r15.png)

You can select the session duration and define the amount of time before a user is logged out. For production, I’d recommend 1 hour, but for development and testing you can use 4 hours or more.

---

### 3. Users

![Users](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ji9zzeb0w9d3pl29o1ik.png)

You can set user information, job-related details, and even custom attributes.

Finally, assign the user to a group.

---

### 4. Groups

![Groups](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x39cqerrtluz3tf4yvzz.png)

---

### 5. SSO

Finally, you’ll have the AWS access portal URL and credentials for the user—whether provided via email or generated as a random password.

![Portal](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hm98ts57hgqlxwbm2r14.png)

You’ll see the accounts and roles that match the permission sets we defined and allocated to the created user.

Another benefit: you can also implement and configure applications, and enable MFA (which I highly recommend for stronger security).

![MFA](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r980upp8vmusnzlyikkv.png)

---

## Conclusion

With AWS SSO, you can create a powerful architecture with a centralized portal to manage users and permissions, while also taking advantage of consolidated billing and guardrails through Organizations and SCPs.

Overall, the benefits are huge: you gain hands-on experience with multi-account projects, keep your learning active, and—most importantly—avoid the risk of experimenting inside your company’s account.

And the best part: **there’s no additional cost** to use it. My recommendation? Treat it as a default best practice. There are no downsides, only plenty of benefits.


**There has never been a better time to be an engineer and create value in society through software.**


- [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa/)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog at [jorgetovar.dev](https://jorgetovar.dev).
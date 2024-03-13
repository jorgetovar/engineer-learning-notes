# Provisioned Concurrency - Reduce Cold Starts in AWS Lambda Functions PART 1

This series of AWS Lambda articles focuses on performance and best practices. This is part 1, where we explore Lambda cold starts, warming a lambda, and provisioned concurrency. In parts 2 and 3, we will discuss Deployment preferences, Powertools, Observability and Auto-scaling Provisioned concurrency using metrics and patterns, all of this with Infrastructure as Code 🧑🏻‍💻.

AWS Lambda functions have become one of the most important cloud services provided by Amazon. They are the cornerstone of many applications, serving as the core business logic for Serverless applications. They are also the default choice for operating and handling asynchronous or simple tasks that don't require a constantly running component. Over time, Lambda has evolved to be a fully event-driven service with numerous triggers and seamless integration with almost any AWS service.

If you want to go straight to the code: 🚀
[GitHub Code example](https://github.com/jorgetovar/provisioned-concurrency-aws-lambda)

However, one challenge remains in our quest for creating reliable and performant applications: Cold Starts. Despite AWS investing heavily to reduce this time, and for many use cases, it's something we can tolerate, it isn't always the case. Sometimes we need a predictable response latency for our product.

## Cold Starts and Latency

A cold start in AWS Lambda refers to the initial process of spinning up a new execution environment for a function when it's invoked for the first time or after a period of inactivity.

To understand this, we need to know the lifecycle of an AWS Lambda function. Here are the steps that need to be performed in order to get a response from our function:

1. Download the code (Cold start)
2. Create a new execution environment (Cold start)
3. Execute initialization code (Cold start)
4. Execute code handler
5. Shutdown

```python
# Initialization code
import boto3
s3_client = boto3.client('s3')

# Code handler
def lambda_handler(event, context):
    
    response = s3_client.list_buckets()
    
    bucket_names = [bucket['Name'] for bucket in response['Buckets']]
    
    return {
        'statusCode': 200,
        'body': bucket_names
    }
```

As illustrated in the code above, we can conceptualize this process as occurring in three phases:

#### 1.Initialization Phase

During this phase, our code is downloaded, the execution environment with extensions and runtime is created, and the only aspect we control is the function initialization code. This code, outside of the handler, executes once during every cold start.

#### 2. Invoke Handler
This phase executes every time we call our lambda function.

#### 3. Shutdown
Environments are shut down after a non-deterministic period of inactivity. In such cases, a cold start occurs upon the next invocation.

As you can deduce, in terms of cold starts, we only have control over the function initialization. Thus, we should aim to take advantage of any additional invocations within the same execution environment, known as a "warm start."

### Warm Start

With multiple concurrent calls, AWS can utilize already provisioned environments, eliminating the need to download the code and run the initialization code again. Paradoxically, increased traffic can lead to improved performance.

![Cold starts](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ama6fsusxwm923c2sk3z.png)

### Deployments

Every time we update our code, it results in a cold start. This behavior ensures that we run the function with the latest code.

### Function Warmers

In the serverless community, there are libraries and in-house mechanisms used to "warm" Lambda via pings. However, this doesn't eliminate all cold starts, and sometimes it doesn't help in production environments when we experience spikes in traffic and need all of our concurrent environments available to serve user requests.

We can use EventBridge rules to keep our Lambda warm, or use tools to simulate traffic. However, I'm not a big fan of this approach. In such cases, it may be worth considering the possibility of having a running container.

## Reducing Cold Starts with Provisioned Concurrency

If our product requires predictable start times and latency, provisioned concurrency is the way to go. Unlike Lambda's on-demand default behavior, we can avoid the initial phase and recurrent tasks of downloading and executing the initialization code.

These environments are provided ahead of time, but remember:
> Everything in software architecture is a trade-off.

We may end up with a higher cost of running our application since we have a running component available instead of getting something on demand.

Provisioned concurrency works in conjunction with Lambda versions instead of using the $LATEST code as we usually do. We also don't need to optimize initialization code since this is executed before creating the execution environment.

We may still have cold starts due to the defined capacity in our provisioned concurrency. Therefore, we can configure the amount of ready-to-use Lambdas in our system.

We can create auto-scaling policies to make this more robust and reduce the cost of operating our application. If we have predictable patterns or metrics, we can reduce the amount of provisioned concurrent executions.

We can also define some rollout and deployment preferences as part of this strategy, for example, using canary or linear deployments.

## Hands-on Example
[Provisioned concurrency repository](https://github.com/jorgetovar/provisioned-concurrency-aws-lambda)
 
Every time that we make a HTTPs call for the very first time we would see the impact of the Cold start, with a response time that seems to be pretty high -> 2 seconds.

```javascript
{
    "message": "Hello community builders - from AWS Lambda!"
}
```

![Cold start example](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/godg3rfg6551372yxdct.png)

We usually get a new Log stream if we update the Lambda.

![Provisioned concurrency Log stream](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ap2j25r4sq0q19op6arf.png)

Finally in the Logs we would see something like this, where we can validate the Init duration in our case it is 824.46 ms

> *REPORT RequestId: e8500c25-5c5f-4ffe-867e-60741c3b1df5 Duration: 18.57 ms Billed Duration: 19 ms Memory Size: 128 MB Max Memory Used: 64 MB **Init Duration: 824.46 ms** XRAY TraceId: 1-65f096ba-200e28165f811e694170deef SegmentId: 13f970807474e003 Sampled: true* 

We can go to the Configuration, Concurrency tab and set up everything.

### Configuration

1. Create a Lambda version
![Version](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xw1gdbzco09bpukdu2hk.png)
![Version Success](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k2ot6j8twwjcfcy46dt4.png)

2. Configure the number of available execution environments
![Provisioned concurrency Configure](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vgeq60bshm63xskiq0vd.png)
![Provisioned concurrency In Progress](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cj2zjtnvljvnv1q95dky.png)


![Provisioned concurrency](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8jyztik9dp86w29f40hn.png)


## Results

After the deployment has been successful, if we make an HTTP call, we can avoid most of the cold starts and see consistent response times of 400 ms.
![Provisioned concurrency result](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8nzh5uk60fm9cpcvid3c.png)

We can implement all of this in our CI/CD and Infrastructure as Code processes, but I wanted to provide an overview with the console so you can feel comfortable playing with the settings and doing a proof of concept before diving deep and deploying this.

## Clean Up

Remove all the resources using SAM framework 


![SAM delete](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8n8l68mm4ih59c66iz7x.png)


## Conclusion

Provisioned concurrency is the preferred solution for production workloads that rely on predictable response times and want to avoid cold starts.

It is important to emphasize that the biggest problems in the software development and delivery process are organizational and people-related. In the end, building applications is a social activity, but the more we solve technical problems and improve communication, the easier it should be to reach production. Happy coding! 🎉

- [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog [jorgetovar.dev](jorgetovar.dev)
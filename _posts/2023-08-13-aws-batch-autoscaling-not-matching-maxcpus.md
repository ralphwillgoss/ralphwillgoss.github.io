---
title: "Why are AWS Batch jobs stuck in the RUNNABLE status, even after an increase in MaxvCpus?"
date: 2023-08-13
categories:
  - aws batch ec2
tags:
  - aws batch ec2
---

After increasing the `MaxvCpus` for your **AWS Batch** [Compute Cluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-batch-computeenvironment-computeresources.html#cfn-batch-computeenvironment-computeresources-desiredvcpus) to run more jobs, you may observe a certain number of jobs being executed, and the rest being stuck in the `RUNNABLE` state.

**AWS Batch** jobs getting queued in a `RUNNABLE` status is a common problem which I've [mentioned before]({% link _posts/2020-12-28-aws-batch-not-starting-when-instancetype-is-optimal.md %}).  
AWS have a [support page](https://repost.aws/knowledge-center/batch-job-stuck-runnable-status) dedicated to this issue.  

To determine what is wrong here, we need to see why the Autoscaling is not working as expected.

# Autoscaling Trouble Shooting
Go to the _Scaling Group_ belonging to your AWS Batch instance.  
In the AWS Console, navigate to `EC2 -> Auto Scaling Groups`.

Find the group belonging to the _Compute Environment_, you are trouble shooting.  
If using [IaC](https://en.wikipedia.org/wiki/Infrastructure_as_code) the Auto Scaling Group will be named similar to your `ComputeEnvironmentName`.

![Auto Scaling Groups List](/assets/images/posts/aws-batch-on-demand/ec2-auto-scaling-groups.png) 

When you have navigated to the **Auto Scaling Group**, click on the **Activity** tab so we can find what has `Failed`.  

_Important:  
Depending on how long ago the error happend, your initial search may find 0 matches.  
Search only works on whats loaded in the UI. Therefore, you may need to page back a few times, to load more records in order to find the "Failed" ones._

![Auto Scaling Group Error](/assets/images/posts/aws-batch-on-demand/auto-scaling-group-error-message.png) 

As you can see from the error below, there is an additional **On-Demand** restriction, that I exceeded for the instance types I'm running. As instructed, we are going to need to submit a **EC2** service quota increase request.

```shell
Launching a new EC2 instance. Status Reason: Could not launch On-Demand Instances.
VcpuLimitExceeded - You have requested more vCPU capacity than your current vCPU limit
of 1958 allows for the instance bucket that the specified instance type belongs to.
Please visit http://aws.amazon.com/contact-us/ec2-request to request an adjustment to this limit.
Launching EC2 instance failed.
```

# Increasing an EC2 Service Quota
To increase an EC2 service quota, in the AWS Console navigate to:  
`Service Quotas -> Amazon Elastic Compute Cloud (Amazon EC2)`

As per the error, we are having issues with the **On-Demand** restrictions.  
Filtering for these should give you a list of these quota types.

![On Demand Restrictions](/assets/images/posts/aws-batch-on-demand/on-demand-service-quotas.png)

I'm using `C`, `D` and `R` types, so the quota **Running On-Demand Standard (A, C, D, H, I, M, R, T, Z) instances** applies.  

Click on the quota name and you will see the details of the quota you want to increase.  
You also get a graph, showing your % use of the current quota.

![Quota View](/assets/images/posts/aws-batch-on-demand/service-quota-ec2-on-demand.png) 

On this page you can now click on `Request quota increase`, and apply for an increase.

Requests to increase service quotas don't receive priority support.  
If you have an urgent request, contact AWS Support.

{: .notice--primary}  
<strong>References:</strong>  
[AWS - Why is my AWS Batch job stuck in RUNNABLE status?](https://repost.aws/knowledge-center/batch-job-stuck-runnable-status)  
[Wikipedia - Definition of IaC (Infrastructure As Code)](https://en.wikipedia.org/wiki/Infrastructure_as_code)
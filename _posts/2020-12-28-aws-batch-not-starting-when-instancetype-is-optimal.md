---
title: "Why is AWS Batch not giving me an instance, when I have instance type set to 'optimal'?"
date: 2020-12-28
categories:
  - aws devops
tags:
  - aws batch
---

When using [AWS Batch](https://docs.aws.amazon.com/batch/) there can be many reasons why your jobs appear stuck in the runnable status.  
So much so, that there's a [whole page in the support section](https://aws.amazon.com/premiumsupport/knowledge-center/batch-job-stuck-runnable-status) dedicated to it.

I recently had an issue where I had selected the `optimal` strategy for the `instanceTypes` on my [Compute environment parameters](https://docs.aws.amazon.com/batch/latest/userguide/compute_environment_parameters.html#compute_environment_compute_resources), yet my jobs were appearing stuck in `runnable`. This didnt make any sense, the reason I choose `optimal` was to allow AWS to grab any instance for me from a range.  

After a chat to the very helpful AWS Support I found out that the documentation was misleading, it says *"choose optimal to pick instance types from the C, M, and R instance families"*.  

What it should says is *"Optimal chooses the best fit of M4, C4, and R4 instance types available in the region"*.  
The CPU and Memory requirements I had set for my container, exceeded what any of those instance types could do.  

The AWS support engineer said a ticket has been logged to update the documentation.

{: .notice--primary}
<strong>References:</strong>  
<https://aws.amazon.com/premiumsupport/knowledge-center/batch-job-stuck-runnable-status>{:target="_blank"}  
<https://stackoverflow.com/questions/48151332/why-are-aws-batch-jobs-stuck-in-runnable>{:target="_blank"}  
<https://stackoverflow.com/questions/52301621/how-do-i-view-aws-batch-compute-environment-errors>{:target="_blank"}
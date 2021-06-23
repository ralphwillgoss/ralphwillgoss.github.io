---
title: "Modifying AWS Batch Job Definition in new console causes jobs to get stuck in Runnable status"
date: 2021-06-23
categories:
  - aws batch ec2
tags:
  - aws batch ec2
---

I provision all my infrastructure using [Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code), in this case [AWS CloudFormation](https://aws.amazon.com/cloudformation/).  
There are times though where I will use the GUI, in this case the AWS Console, to tweak things to help with experiments and iteration speed.  

I recently made a change to an AWS Batch Job Definition which inadvertedly caused all my jobs to get stuck in _Runnable_ status.  
Batch Jobs getting stuck in a _Runnable_ status is a known issue with AWS Batch and there's a [AWS guide to trouble shooting it](https://aws.amazon.com/premiumsupport/knowledge-center/batch-job-stuck-runnable-status/).  

However, I did not expect changing some container path mappings in a job definition, to cause any issues.  
After sometime trouble shooting, I discovered a bug in the New Batch console UI.  

I could reproduce the bug as follows.

Looking at the **New Batch experience** console, my job definition looked fine:  
![New Batch Job Definition](/assets/images/posts/aws-batch-ui-bug/new-batch-job-definition.png)

However, after clicking around for a while I decide to disable the **New Batch experience**.  
My initial change was Revision 18 but I don't remember changing the job type to be _Multi-node_?  
![Old Batch Job Definition](/assets/images/posts/aws-batch-ui-bug/old-batch-job-definition.png)

So I check the New Batch Job Requirements UI to see if multi node is enabled, but its not:  
![New Batch Job Requirements](/assets/images/posts/aws-batch-ui-bug/new-batch-job-definition-multinode.png)

I then switch back to the old Batch console Job Requirements, to see if its enabled there too but its not:  
![Old Batch Job Requirements](/assets/images/posts/aws-batch-ui-bug/old-batch-job-definition-multinode.png)

However, the Old Batch console experience shows me something interesting.  
I've never assigned 4 GPU's, yet the vCPU and Memory requirements are blank?  
![Old Batch Job Requirements](/assets/images/posts/aws-batch-ui-bug/old-batch-job-requirements.png)

The fix I discovered was to **Create a new Revision** using the Old Batch console experience, entering my CPU and Memory requirements again. Upon creating this new revision, I noticed that vCPUs and Memory were now showing correctly in the Old Batch console experience.  
![Old Batch Job Definition](/assets/images/posts/aws-batch-ui-bug/old-batch-job-definition-correct.png)

Next, I saw EC2 provisioning an instance for my jobs that were stuck and eventually they were executed successfully.

Bug reported on the [AWS Batch Forums](https://forums.aws.amazon.com/thread.jspa?threadID=342189).

{: .notice--primary}
<strong>References:</strong>  
[Infrastructure as Code](https://en.wikipedia.org/wiki/Infrastructure_as_code){:target="_blank"}  
[AWS CloudFormation](https://aws.amazon.com/cloudformation/){:target="_blank"}  
[Why is my AWS Batch job stuck in RUNNABLE status?](https://aws.amazon.com/premiumsupport/knowledge-center/batch-job-stuck-runnable-status/){:target="_blank"}  

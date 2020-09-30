---
title: "LambdaDeployFunction - [error]Error: Function:my-function does not exist, cannot update code only"
date: 2020-09-11
categories:
  - azure devops aws
tags:
  - aws azure-devops aws-toolkit-azure-devops
---

Today I discovered an issue with the [AWS Toolkit for Azure DevOps](https://aws.amazon.com/vsts/){:target="_blank"}, specifically the [AWS Lambda Deploy Function](https://docs.aws.amazon.com/vsts/latest/userguide/lambda-deploy.html){:target="_blank"}.  
The task `task: LambdaDeployFunction@1` will incorrectly report that your function cannot be found, even if it does exist.  

```bash
# note: AWS Toolkit for Azure DevOps version: 1.7.0
##[error]Error: Function:my-function does not exist, cannot update code only
```

I tried another way of accessing the function using: 
```
aws lambda get-function --function-name my-function
```

This then produced the following error:  
```bash
An error occurred (AccessDeniedException) when calling the GetFunction operation: User: arn:aws:sts::myrole is not authorized
to perform: lambda:GetFunction on resource: arn:aws:lambda:<path>:function:my-function
```

So I actually had a permissions error, if we looks at the code we can see that the function [testForFunction](https://github.com/aws/aws-toolkit-azure-devops/blob/5c3ea378838f82e7aa81842404d944138f033ed3/Tasks/LambdaDeployFunction/TaskOperations.ts#L212){:target="_blank"} surpresses all exceptions.
This issue has been raised on [aws-toolkit-azure-devops on github](https://github.com/aws/aws-toolkit-azure-devops/issues/371){:target="_blank"}.
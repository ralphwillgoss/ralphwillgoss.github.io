---
title: "Azure DevOps and AWS ToolKit task to GitHub source reference"
date: 2021-06-14
categories:
  - aws github azuredevops aws-toolkit
tags:
  - aws github azuredevops aws-toolkit
---
When searching for the source for the AWS Toolkit for Microsoft Azure DevOps, you won't find direct references to the docs or the source when using the Azure DevOps task naming convention.

This is because the Azure DevOps task name is derived compared to its AWS ToolKit task name:  
e.g. `CloudFormationCreateOrUpdateStack@1` vs `AWS CloudFormation Create/Update Stack`  

I created this reference page to quicky refer to the source and docs for the AWS Toolkit for Microsoft Azure DevOps, when I need to investigate a behaviour or issue.

## Docs and Source Entry Points
Docs: [AWS Toolkit for Microsoft Azure DevOps](https://docs.aws.amazon.com/vsts/latest/userguide/welcome.html)  
GitHub: [https://github.com/aws/aws-toolkit-azure-devops]()

## Deployment Tasks
[Docs: AWS CodeDeploy Application Deployment](https://docs.aws.amazon.com/vsts/latest/userguide/codedeploy-deployment.html)  
[GitHub: CodeDeployDeployApplication@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/CodeDeployDeployApplication)

[Docs: AWS CloudFormation Create/Update Stack](https://docs.aws.amazon.com/vsts/latest/userguide/cloudformation-create-update.html)  
[GitHub: CloudFormationCreateOrUpdateStack@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/CloudFormationCreateOrUpdateStack)

[Docs: AWS CloudFormation Delete Stack](https://docs.aws.amazon.com/vsts/latest/userguide/cloudformation-delete-stack.html)  
[GitHub: CloudFormationDeleteStack@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/CloudFormationDeleteStack)

[Docs: AWS CloudFormation Execute Change Set](https://docs.aws.amazon.com/vsts/latest/userguide/cloudformation-execute-changeset.html)  
[GitHub: CloudFormationExecuteChangeSet@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/CloudFormationExecuteChangeSet)

[Docs: AWS Elastic Beanstalk Create Version](https://docs.aws.amazon.com/vsts/latest/userguide/elastic-beanstalk-createversion.html)  
[GitHub: BeanstalkCreateApplicationVersion@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/BeanstalkCreateApplicationVersion)

[Docs: AWS Elastic Beanstalk Deploy Application](https://docs.aws.amazon.com/vsts/latest/userguide/elastic-beanstalk-deploy.html)  
[GitHub: BeanstalkDeployApplication@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/BeanstalkDeployApplication)

Docs: Amazon ECR Pull *not available  
[GitHub: ECRPullImage@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/ECRPullImage)

[Docs: Amazon ECR Push](https://docs.aws.amazon.com/vsts/latest/userguide/ecr-pushimage.html)  
[GitHub: ECRPushImage@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/ECRPushImage)

[Docs: AWS Lambda Deploy Function](https://docs.aws.amazon.com/vsts/latest/userguide/lambda-deploy.html)  
[GitHub: LambdaDeployFunction@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/LambdaDeployFunction)

[Docs: AWS Lambda .NET Core](https://docs.aws.amazon.com/vsts/latest/userguide/lambda-netcore-deploy.html)  
[GitHub: LambdaNETCoreDeploy@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/LambdaNETCoreDeploy)

[Docs: AWS Lambda Invoke Function](https://docs.aws.amazon.com/vsts/latest/userguide/lambda-invoke.html)  
[GitHub: LambdaInvokeFunction@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/LambdaInvokeFunction)

## General Purpose Tasks
[Docs: AWS CLI](https://docs.aws.amazon.com/vsts/latest/userguide/aws-cli.html)  
[GitHub: AWSCLI@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/AWSCLI)

[Docs: AWS Tools for Windows PowerShell Script](https://docs.aws.amazon.com/vsts/latest/userguide/awspowershell-module-script.html)  
[GitHub: AWSPowerShellModuleScript@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/AWSPowerShellModuleScript)

[Docs: AWS Shell Script](https://docs.aws.amazon.com/vsts/latest/userguide/awsshell.html)  
[GitHub: AWSShellScript@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/AWSShellScript)

[Docs: Amazon S3 Download](https://docs.aws.amazon.com/vsts/latest/userguide/s3-download.html)  
[GitHub: S3Download@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/S3Download)

[Docs: Amazon S3 Upload](https://docs.aws.amazon.com/vsts/latest/userguide/s3-upload.html)  
[GitHub: S3Upload@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/S3Upload)

[Docs: AWS Send SNS or SQS Message](https://docs.aws.amazon.com/vsts/latest/userguide/send-message.html)  
[GitHub: SendMessage@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SendMessage)

[Docs: AWS Secrets Manager Create/Update Secret](https://docs.aws.amazon.com/vsts/latest/userguide/secretsmanager-create-update.html)  
[GitHub: SecretsManagerCreateOrUpdateSecret@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SecretsManagerCreateOrUpdateSecret)

[Docs: AWS Secrets Manager Get Secret](https://docs.aws.amazon.com/vsts/latest/userguide/secretsmanager-getsecret.html)  
[GitHub: SecretsManagerGetSecret@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SecretsManagerGetSecret)

[Docs: AWS SSM Get Parameter](https://docs.aws.amazon.com/vsts/latest/userguide/systemsmanager-getparameter.html)  
[GitHub: SystemsManagerGetParameter@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SystemsManagerGetParameter)

[Docs: AWS SSM Set Parameter](https://docs.aws.amazon.com/vsts/latest/userguide/systemsmanager-setparameter.html)  
[GitHub: SystemsManagerSetParameter@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SystemsManagerSetParameter)

[Docs: AWS SSM Run Command](https://docs.aws.amazon.com/vsts/latest/userguide/systemsmanager-runcommand.html)  
[GitHub: SystemsManagerRunCommand@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SystemsManagerRunCommand)
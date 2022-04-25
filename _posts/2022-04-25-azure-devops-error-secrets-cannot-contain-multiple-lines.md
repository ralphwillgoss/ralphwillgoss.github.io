---

title: "How do you fix the Azure DevOps Error: Secrets cannot contain multiple lines?"
date: 2022-04-25
categories:
  - aws azuredevops secrets-manager
tags:
  - aws azuredevops secrets-manager
---

When using the [SecretsManagerGetSecret](https://docs.aws.amazon.com/vsts/latest/userguide/secretsmanager-getsecret.html) task in **Azure DevOps**, to retrieve a secret from **AWS Secrets Manager**, you may encounter the following error message:
```powershell
task: SecretsManagerGetSecret@1
..<snip>
Reading value for secret 'secret-name'.
##[error]Error: Secrets cannot contain multiple lines
```

Looking in `Secrets Manager` in the **AWS Console** will not reveal the problem.  
Everything will look fine, even when viewing the secret in `plain text` mode.  

To see the cause of the error, we need to use the following command to retrieve the secret in its raw form:
```shell
aws secretsmanager get-secret-value --secret-id <value>
```

You will see an output similar to the below: 
```json
"SecretString": "{\r\n \"some-key\": some-value\",\r\n}" ..<snip>
```
Notice the `\r\n` characters, which were not visible in the `Secrets Manager` plain text  mode.  

To fix, remove the secrets and re-add them, ensuring you do not copy any control characters.


{: .notice--primary}
<strong>References:</strong>  
[AWS CLI: secretsmanager get-secret-value](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/secretsmanager/get-secret-value.html)  
[Docs: AWS Secrets Manager Get Secret](https://docs.aws.amazon.com/vsts/latest/userguide/secretsmanager-getsecret.html)  
[GitHub: SecretsManagerGetSecret@1](https://github.com/aws/aws-toolkit-azure-devops/tree/master/src/tasks/SecretsManagerGetSecret)
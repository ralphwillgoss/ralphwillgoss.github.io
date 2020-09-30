---
title: "Permission error when deploying a Lambda from a zip file in S3 - [Errno 13] Permission denied"
date: 2020-09-14
categories:
  - azure devops aws
tags:
  - aws azure-devops aws-toolkit-azure-devops
---

Today I received the following error when trying to deploy from Azure DevOps a lambda to AWS via a zip file in S3:
```python
[ERROR] PermissionError: [Errno 13] Permission denied: '/var/task/inspector.py'
Traceback (most recent call last):
  File "/var/lang/lib/python3.7/imp.py", line 300, in find_module
    with open(file_path, 'rb') as file:
```

I was compressing the inspector.py using the Compress-Archive powershell function:
```yaml
- task: PowerShell@2
    displayName: zip lambda
    inputs:
      targetType: inline
      script: Compress-Archive -Path inspector.py -DestinationPath inspector.zip
      errorActionPreference: 'stop'
```

The issues is that the `powershell` command `Compress-Archive`, doesn't archive the files with the correct persmissions and there's no easy override.
The solution I used was to use `python`, its simple and by default sets the correct permissions:
```yaml
- task: PythonScript@0
    displayName: zip inspector.py
    inputs:
      scriptSource: inline
      script: |
        from zipfile import ZipFile
        with ZipFile('inspector.zip', 'w') as zf:
          zf.write('inspector.py')
```

{: .notice--primary}
<strong>References:</strong>  
<https://stackoverflow.com/questions/46076543/permission-denied-after-uploading-aws-lambda-python-zip-from-s3>{:target="_blank"}
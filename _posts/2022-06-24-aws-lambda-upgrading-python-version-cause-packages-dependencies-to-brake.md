---
title: "AWS Lambda - Why does upgrading the python version cause the packages dependencies to brake?"
date: 2022-06-24
categories:
  - aws lambda python
tags:
  - aws lambda python
---

If you are upgrading your AWS python Lambda's from 3.7 to 3.8 or 3.9, you may find your previously packaged runtime dependencies no longer work.

A common thing developers package is the [requests](https://requests.readthedocs.io/en/latest/) library, so you would see an an error like:
```python
cannot import module requests from packages.requests
or 
Unable to import module 'lambda_function': No module named lambda_function
```

<br/>
This appears to be in part caused by a change in the underlying [python runtime](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html) used for Lambdas.
![AWS python runtime](/assets/images/posts/aws-lambda-runtime/aws-python-runtime.png)


The [AWS python package documentation](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html) has been updated to reflect this change but its very subtle and can be easily overlooked. The documentation uses a `package` (no s) directory for installing the dependency, **not for including the dependency**.

Here's the documentation section in question:  
![AWS python package](/assets/images/posts/aws-lambda-runtime/aws-python-package.png)

You need to ensure that the external dependencies are in the root, not the `packages` folder.  
Your directory structure including the `requests` dependencies, would now look like the following:
```python
lambda_handler.py
/requests
```

## Addendum: Importing Indirect Dependencies
Its worth noting that some people have relied on the requests package via boto as this popular [Stack Overflow post](https://stackoverflow.com/questions/40741282/cannot-use-requests-module-on-aws-lambda) would indicate.
This is not a good practice:
- you have less control over the version you use
- the dependency can decide to remove the package you are importing indirectly

{: .notice--primary}
<strong>References:</strong>  
[python requests Documentation](https://requests.readthedocs.io/en/latest/)  
[AWS Lambda python deployment package](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html)  
[AWS Lambda python runtimes](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html)  
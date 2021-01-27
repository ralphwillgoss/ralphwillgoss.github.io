---
title: "Why is CloudFormation not updating my Stacks Description?"
date: 2021-01-27
categories:
  - aws devops
tags:
  - aws cloudformation
---
I was trying to update the `Description` of a CloudFormation stack:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "This is the description I wanted to update as its viewable within the AWS Console"
Resources:
      <snip resources>
```

However, I was presented with the following warning:

```shell
##[warning]WARNING: no changes were detected for the template or change set. The stack was not updated.
```

So though the stack has changed, changing the **Description** by itself is not enough to trigger CloudFormation to update.  

A work around is to add a variable in your template within the `Resources` section.  
This will be detected as a change, so it "looks" different than what you curently have.  

CloudFormation will then compare the current state to your updated definition and perform an update, including the description.  
When finished, remove the variable and redeploy.
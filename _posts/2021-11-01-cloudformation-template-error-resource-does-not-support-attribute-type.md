---
title: "AWS CloudFormation Template error - Resource does not support attribute type"
date: 2021-11-01
categories:
  - aws cloudformation
tags:
  - aws cloudformation
---
A common thing you might want to do when using CloudFormation, is to refer to a S3 Bucket and gets its `BucketName` as an input for another resource using `!GetAtt`. In my case, I'm setting the `BucketName` as an environment variable to be used by a Lambda:

```shell
Environment:
  Variables:
    S3_BUCKET: !GetAtt S3Bucket.BucketName
    or
    S3_BUCKET: !GetAtt [S3Bucket, BucketName]
```

However, running this will produce the following error:
```shell
message: 'Template error: resource S3Bucket does not support attribute type BucketName in Fn::GetAtt',
```

# Solution - when to use !Ref vs !GetAtt
Its not obvious at first but if you read the [documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html) on how `!Ref` works, its worth noting its behaviour is not consistent across all resources:  
- if you use `!Ref` to refer to a [AWS::SNS::Topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic.html), it returns the `ARN` of the resource
- if you use `!Ref` to refer to a [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html), it returns the `BucketName`  

So in the end my solution looks like this:
```shell
Environment:
  Variables:
    S3_BUCKET_NAME: !Ref S3Bucket
```

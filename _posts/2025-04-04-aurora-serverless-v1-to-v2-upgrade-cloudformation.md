---
title: "Migrating Aurora Postgres Serverless v1 to v2 using CloudFormation"
date: 2025-04-04
categories:
  - aws aurora postgres cloudformation
tags:
  - aws aurora postgres cloudformation
---

When migrating your **Aurora Postgres Serverless** from **v1** to **v2**, **AWS** provides a comprehensive guide - 
   ["Migrating to Aurora Serverless v2"](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2.upgrade.html#aurora-serverless-v2.upgrade-from-serverless-v1-procedure).

As of March 31st 2025, AWS had noted that they were going to begin automatically migrating `Resource`'s from **v1** to **v2**.  
This may catch some people out if they are using **CloudFormation (CF)**, as at the time of writing this upgrade is not compatible with the **v1** **CF** you have in source control.

After following the [AWS Aurora Serverless V2 upgrade guide](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2.upgrade.html#aurora-serverless-v2.upgrade-from-serverless-v1-procedure), even if you match your **CF** to what [drift detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html) shows you, when you perform an update you will see the following error:
``` yaml
CloudFormation cannot update a stack when a custom-named resource requires replacing.
Rename <db cluster name> and update the stack again.
```
This is because some of the params that need changing, require `Replacement`, as opposed to `Some interruptions` or `No interruption`.

After going through this process several times, I made some notes as it was a great example of having to migrate complex `Resource`'s which are stored in **CF**.  In particular, when you need to update your **CF** to match the newly created **v2** Aurora `DBCluster`.

Below are my additional notes when upgrading the `DBCluster` from **v1** to **v2** and migrating the **CF**.

# Prerequisites
While the process does an in place upgrade of the database, there are a few operational things to be aware of:
- Ensure you have the appropriate database backups
  - The upgrade process creates a DB snapshot, as part of the migration process
- The [default port for Aurora Postgres](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbcluster.html#cfn-rds-dbcluster-port) **v1** is `5432`
   - If not explicitly set in your **CF**, this has changed with **v2** to port `3306`, when using the new `EngineMode:provisioned` for **v2**
- The migration process does not change the `master` database password
   - If you are setting this using **CF**, dependencies may inadvertedly cause an update to it

# Migrating to Aurora Serverless v2
The upgrade paths differ considerably depending on the version of the database you are using.  
It's well documented but does require careful reading.

As I'm running Postgres `v13.x`, the initial process is very easy and I just followed the instructions under the guide for the section: ["To upgrade an Aurora Serverless v1 cluster running Aurora PostgreSQL version 13"](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2.upgrade.html#aurora-serverless-v2.upgrade-from-serverless-v1-procedure).

Here are my additional notes to the provided guide:

1. "Convert the Aurora Serverless ..."  
   - This part of the process is variable and depending on your database size can take minutes, to hours

2. "Add an Aurora Serverless v2 reader ..."  
   - Remember to select instance type of `Serverless v2`
   - Instance name is globally unique
   - Use the guide to match your current instance `Max` and `Min`

3. "Fail over to the Aurora Serverless ..."  
   - When failing over there some text is displayed in the AWS Console to this occurring but it can be missed.  
     Watch for your new `v2 serverless` instance to become the `writer`.

# Updating the stack using Infrastructure Composer
Using `Infrastructure Composer` makes the process very easy and gives clear visibility as to exactly what **CF** changes you are applying. Hat tip to the guys at **AWS Support**, who showed me the little hack of using `Infrastructure Composer` to modify the **CF** , then store it in **S3** "temporarily" for the **Import Operation**. 

So below, when I refer to _Updating the stack_ this method is what is implied.  
Open the stack you want to modify using `AWS Console -> CloudFormation` and then click Update:

 ![Infrastructure Composer](/assets/images/posts/aurora-v1-upgrade/cloudformation-update-stack.png)

 Select **Edit in Infrastructure Composer**

 ![Infrastructure Composer](/assets/images/posts/aurora-v1-upgrade/cloudformation-edit-stack.png)

- Once `Infrastructure Composer` opens up, you will see a visual representation of your resources  
- Click on "Template" button to view the **CF** `Resources`, and toggle wether you want to view in `yaml` or `json`
  - This guide assumes you are using `yaml`

# Resolve CloudFormation stack drift with an import operation
AWS provide a guide to [resolving stack drift](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-resolve-drift.html), so you can update your **CF** to match that of the `Resource` in AWS. As part of the upgrade of Aurora Serverless from **v1** to **v2**, you will get some stack drift.

Here are my additional notes to each section of the **AWS** guide:  

1. "Update stack with Retain deletion policy"  
   _Note: the following notes, help from step `3` in the AWS guide_

   Update the stack and as per the instructions, add a `DeletionPolicy` attribute and set it to `Retain` for any `DBCluster` resources. This ensures the existing `DBCluster` resources are retained rather than deleted, when we temporarily remove them from the stack in a later step.
   - Use `spaces` not `tabs` for indentation when modifying the `yaml`, or the validation will fail  
   - Use the `Validate` button and then the `Update template` button, to apply the changes
   - When viewing the **"Change set preview"** you should see the retained `DBCluster` resources being marked as `replacement = false`

   You'll now be redirected back to the _Update stack_ **CF** screen, you can now continue from step `5` in the AWS guide, where we now apply the **CF** changes.

2. "Remove drifted resources, related parameters, and outputs"  
   - Validate that the `DeletionPolicy` has been applied by viewing the current **Template** in **CF**:

     ![Template tab](/assets/images/posts/aurora-v1-upgrade/cloudformation-template-tab.png)
   
   - Update the stack and search for all references to your `DBCluster` resources and replace any dynamic values with the actual values. You can find these values on the **Resources** tab:

     ![Resources tab](/assets/images/posts/aurora-v1-upgrade/cloudformation-resources-tab.png)
   - Now comment out the `DBCluster` resources from the template (Ctrl + /), we will need these again later
   - Use the `Validate` button and then the `Update template` button, to apply the changes
   
   You'll will now follow the same process as before in the AWS guide, to apply the **CF** changes.

   Go to `AWS Console -> CloudFormation -> Events` and you should see a **Delete Skipped** for the `DBCluster` resources you just commented out.

   ![Delete Skipped](/assets/images/posts/aurora-v1-upgrade/cloudformation-delete-skipped.png)

3. "Update template to match the live state of your resources"  
  This is where we now import the `DBCluster` resources and get the **CF** to match what AWS has.
  
   **Preparing CF Resources for import**
    - Update the stack and uncomment all references to your `DBCluster`
    - Replace the [AWS::RDS::DBCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbcluster.html#cfn-rds-dbcluster-scalingconfiguration) `ScalingConfiguration` with the new `ServerlessV2ScalingConfiguration` property and set `MaxCapacity` and `MaxCapacity`
    - Update the `EngineMode` to `provisioned`
    - Update the `EngineVersion` to match the existing version being used in AWS
      - This can drift if you have the automatic minor upgrade feature turned on
    - Ensure these changes are applied to your source controlled version of the `Resource`'s
    - Use the `Validate` button and then the `Update template` button to save the CF, but **do not** apply them
    - Copy the text for the location of where the updated template is stored in S3, we will use this reference to import the resource:

    ![S3 Url](/assets/images/posts/aurora-v1-upgrade/cloudformation-s3-url.png)

   **Import Resources**  
    As per the AWS guide, follow the instructions to import using **Import resources into stack** menu option
    - Provide the S3 link we copied before, when specifying location for importing of the resources
    - You'll need to provide the current value for your `DBInstanceId`. The import operation needs to match the imported resource, to the one that already exists as we don't want to create it again
    - One the last page, the **Changeset preview** should show an Action of `Import`  

    Once completed, you should see your `DBCluster` resources marked as Imported in the **CF** Events tab

   **Replace hard coded references with original CF references**  
    Once the resources are imported successfully, update the stack and replace the references that we hard coded in step `2` with their original dynamic **CF** reference.
    - Use the `Validate` button and then the `Update template` button to save the CF, then apply the changes
    - Once completed perform a [stack drift detection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html), and you should see no differences

4. Add a new `DBInstance` and deploy updated **CF**  
   In step **3** your source controlled **CF** should have been updated to match what we have now in AWS.  
   Add to your **CF** a [DBInstance](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbinstance.html) `Resource`, currently all the instances have been manually created.

   We can now run the deployment pipeline, to deploy our updated **CF** and prove what is in source control and in AWS are now in sync. As the **CF** now matches the `Resource` in AWS, you should receive no errors.

5. Remove manually created additional instances  
   As part of the **Migrating to Aurora Serverless v2** upgrade process, some additional instances would have been created. Before removing any, ensure you have failed over to the new instance created by the **CF** in step `4`.

Your Aurora Serverless **v1** is now migrated to **v2** and your **CloudFormation** should now match this upgrade.  
Feedback on this guide is welcome.

{: .notice--primary}  
<strong>References:</strong>  
[AWS - Aurora Serverless-v2 Upgrade](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2.upgrade.html#aurora-serverless-v2.upgrade-from-serverless-v1-procedure)  
[AWS - Detect Stack Drift](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html)  
[AWS - Resolve Stack Drift](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-resolve-drift.html)
[AWS::RDS::DBCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbcluster.html
[AWS::RDS::DBInstance](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbinstance.html)

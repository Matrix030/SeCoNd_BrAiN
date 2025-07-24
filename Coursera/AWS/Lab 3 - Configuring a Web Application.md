# AWS Cloud Technical Essentials: Configure a Web Application to use an Amazon S3 Bucket and Amazon DynamoDB Table
**SPL-CX-100-CETEWA-1 - Version 1.0.1**

© 2025 Amazon Web Services, Inc. or its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited. All trademarks are the property of their owners.

Note: Do not include any personal, identifying, or confidential information into the lab environment. Information entered may be visible to others.

Corrections, feedback, or other questions? Contact us at [_AWS Training and Certification_](https://support.aws.amazon.com/#/contacts/aws-training).

## Lab overview

This lab guides you through configuring a web-based employee directory application by integrating Amazon S3 for photo storage and DynamoDB for employee data. You’ll create and configure an S3 bucket to store employee images, set up appropriate bucket policies for access, and upload employee photos. Then you’ll create a DynamoDB table to store employee information, and learn how to manage employee records through both the web interface and AWS Console. By the end of the lab, you’ll have a fully functional employee directory that combines cloud storage and database services to manage employee information and images.

### Objectives

After completing this lab, you will be able to:

- Create an Amazon Simple Storage Service (Amazon S3) Bucket
- Create an S3 bucket policy
- Modify an Application to Use an S3 Bucket
- Upload an Object to an S3 Bucket
- Create an Amazon DynamoDB table
- Test an Application Using an Application Web Interface
- Manage Existing DynamoDB Items Using the AWS Management Console
- Create Items in a DynamoDB Table Using the AWS Management Console

### Technical knowledge prerequisites

This lab requires:

- Notebook computer with Wi-Fi and Microsoft Windows, macOS, or Linux (Ubuntu, SuSE, or Red Hat)
- Administrator access (Microsoft Windows users)
- Internet browser, such as Chrome, Firefox, or Internet Explorer 9 or later

**Note:** Tablet devices cannot access the lab environment, although they can display student guides.

## Task 1: Create an Amazon Simple Storage Service (Amazon S3) Bucket
To support the lab, we created some required resources for you. These resources include a VPC with two public subnets in two different Availability Zones, an Internet Gateway, a Route Table with a route to the Internet, and an EC2 instance hosting your employee directory application. See below for a resource overview:
![[Pasted image 20250724102533.png]]


![[Pasted image 20250724103423.png]]
![[Pasted image 20250724103518.png]]

First, you must address the S3 configuration. To fix this, you create an S3 bucket and upload images to it. You also configure a policy to allow the application IAM role to access the S3 bucket, allowing the application to display the images.

In this task, you create an S3 bucket. Every object in Amazon S3 is stored in an S3 bucket. When you create your bucket, make sure that you create the bucket in your specific lab Region. You can find the lab region on the instructions page, in the left pane.
![[Pasted image 20250724103958.png]]
![[Pasted image 20250724104233.png]]
![[Pasted image 20250724104241.png]]

![[Pasted image 20250724104254.png]]
![[Pasted image 20250724104352.png]]

## Task 3: Modify the Application to Use the S3 Bucket

In this task, you configure your application to use the bucket as a source for employee images.

17. Return to the browser tab with the Employee Directory application.
    
18. In the **Administration** section, choose the **Configuration**.
    

The **Configuration Settings** for the Employee Directory should look like this:
![[Pasted image 20250724104405.png]]

![[Pasted image 20250724104457.png]]

Your **Configuration Settings** page should now look like the following:
![[Pasted image 20250724104612.png]]

## Task 4: Upload an Object to an S3 Bucket

You created a bucket and granted permission to it from your EC2 Instance. Next, upload objects (images) to the Amazon S3 Bucket. An _object_ can be any kind of file – text, photo, video, .zip, etc. When you add an object to an S3 bucket, you can include custom _metadata_ with the object and set _permissions_, providing granular access control.

In this task, you upload objects to your S3 bucket.
![[Pasted image 20250724104750.png]]
![[Pasted image 20250724105342.png]]


## Task 5: Create an Amazon DynamoDB Table

Now that the S3 configuration is complete, the employee directory application is almost ready. But first, you must configure the application to present employee data using Amazon DynamoDB. In this task, you create a DynamoDB table to store all of your employee data for the employee directory application.
![[Pasted image 20250724105432.png]]

## Task 6: Test the Application Using the Application Web Interface

37. In your browser, go to the browser tab with the web application, and refresh the page.
![[Pasted image 20250724105748.png]]
![[Pasted image 20250724110005.png]]

## Task 7: Manage Existing DynamoDB Items Using the AWS Management Console

In the next task, you validate that the your new employee data exists in your DynamoDB table and use the AWS Management Console to edit the data.

45. Return to the browser table with your DynamoDB table.
    
46. In the **Tables** section, choose the **Employees** link.
    
47. Review the **Employees** table details, including the Overview and Indexes tabs.
![[Pasted image 20250724110112.png]]

## Task 8: Create Items in a DynamoDB Table Using the AWS Management Console

In this task, you use the AWS Management Console to create DynamoDB items in your table.

51. Go to your browser tab with the DynamoDB table.
![[Pasted image 20250724110730.png]]

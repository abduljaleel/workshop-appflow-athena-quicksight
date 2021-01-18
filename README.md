# AWS Appflow, Athena and Quicksight in Action - Workshop

## Introduction to AWS Appflow, Athena and Quicksight

*   Amazon AppFlow is a fully managed integration service that helps to transfer data between Software-as-a-Service (SaaS) applications like Salesforce, Marketo, Slack, and ServiceNow, and AWS services like Amazon S3 and Amazon Redshift, in just a few clicks.
*   Amazon Athena is the Amazon Web Services (AWS) service that allows to directly query files stored in S3 using SQL.
*   Amazon Quicksight is an AWS dashboarding service. It has a user-friendly drag and drop interface to create charts and full dashboards in less than an hour.

## Requirements

### AWS Account

In order to complete this workshop, you’ll need access to an AWS account. Your access needs to have sufficient permissions to create resources in Appflow, Athena, S3 and Quicksight. If you currently don’t have an AWS account, you can create one \[here\](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account)

### Slack Account

You'll need a slack account and a workspace to install the Slack app that we are creating as part of this workshop. You may not have Slack app install permissions for a workspace that you have joined unless the administrator has turned on the permissions. So please have a workspace ready to install the Slack app. 

Once you have AWS & Slack accounts, you can start the workshop.

## Architecture

In this workshop, you will be integrating Slack with Amazon Appflow and transferring data from Slack to S3 bucket.  In the next stage, create Amazon Athena table and query the data using SQL.  In the final stage of the workshop, you will be creating charts in Amazon Quicksight.

![image](images/arch.png)

## Preliminary Setup

In the workshop, you will be using Amazon S3 bucket for below purposes:

*   Datalake for storing Slack data. 
*   Saving query results from Athena (This is a pre-requisite for using Athena)

These S3 buckets must be created prior to starting the workshop.  

### Create S3 Bucket for storing slack data

![image](images/s3-datalake.png)

You can leave all the fields as default and add some tags as best practice.  Select region as Sydney and you can use the same region across all other resources in this workshop. 

### Create S3 Bucket for saving Athena query results

![image](images/s3-athenaresults.png)

You can leave all the fields as default and add some tags as best practice.  Select region as Sydney and you can use the same region across all other resources in this workshop. 

## Cleanup

At the end of every workshop, you may wish to clean up the stuff you have created so you do not incur additional costs.

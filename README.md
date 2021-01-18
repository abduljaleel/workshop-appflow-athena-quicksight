# AWS Appflow, Athena and Quicksight in Action - Workshop

## Introduction to AWS Appflow, Athena and Quicksight

*   Amazon AppFlow is a fully managed integration service that helps to transfer data between Software-as-a-Service (SaaS) applications like Salesforce, Marketo, Slack, and ServiceNow, and AWS services like Amazon S3 and Amazon Redshift, in just a few clicks.
*   Amazon Athena is the Amazon Web Services (AWS) service that allows to directly query files stored in S3 using SQL.
*   Amazon Quicksight is an AWS dashboarding service. It has a user-friendly drag and drop interface to create charts and full dashboards in less than an hour.

## Requirements

### AWS Account

In order to complete this workshop, you’ll need access to an AWS account. Your access needs to have sufficient permissions to create resources in Appflow, Athena, S3 and Quicksight. If you currently don’t have an AWS account, you can create one [here](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account)

### Slack Account

You'll need a slack account and a workspace to install the Slack app that we are creating as part of this workshop. You may not have Slack app install permissions for a workspace that you have joined unless the administrator has turned on the permissions.

If you do not have a Slack workspace, create a new workspace following instructions [here](https://slack.com/intl/en-au/help/articles/206845317-Create-a-Slack-workspace)

## Architecture

In this workshop, you will be integrating Slack with Amazon Appflow and transferring data from Slack to S3 bucket.  In the next stage, create Amazon Athena table and query the data using SQL.  In the final stage of the workshop, you will be creating charts in Amazon Quicksight using Amazon Athena as a source for dataset.

![image](images/arch.png)

You will be analysing messages that are created in Slack channel as part of CI/CD pipeline deployment notifications.

## Preliminary Setup

In the workshop, you will be using Amazon S3 bucket for below purposes:

*   Datalake for storing Slack data.
*   Saving query results from Athena (This is a pre-requisite for using Athena)

These S3 buckets must be created prior to starting the workshop.  

### Create S3 Bucket for storing slack data

![image](images/s3-datalake.png)

You can leave all the fields as default and add some tags as best practice. Select region as Sydney and remember to use the same region across all other resources in this workshop.

### Create S3 Bucket for saving Athena query results

![image](images/s3-athenaresults.png)

You can leave all the fields as default and add some tags as best practice. Select region as Sydney and remember to use the same region across all other resources in this workshop.

### Verify the new S3 buckets

![image](images/s3-buckets.jpg)

Once these S3 buckets are created, you are ready to start the workshop.

## Create and Install Slack App

In this step, you will create a slack app by following below instuctions:

*   Sign in to your Slack workspace where you’d like to install the new app, or \[create a new workspace\](https://slack.com/intl/en-au/help/articles/206845317-Create-a-Slack-workspace. Name the workspace as 'apt-workshop-workspace'.
*   Create a Slack app named 'APTWorkshopApp' from [here](https://api.slack.com/docs/sign-in-with-slack#sign-in-with-slack__details__create-your-slack-app-if-you-havent-already)

![image](images/slack-app-new.png)

*   Select the workspace where you want to install the app.
*   After you create the app, in the navigation pane, under Features, choose OAuth & Permissions.
*   For Redirect URLs, enter ap-southeast-2.console.aws.amazon.com/appflow/oauth and save.

![image](images/slack-oath.png)

*   Set the following user token scopes:
    *   channels:history
    *   channels:read
    *   groups:history
    *   groups:read
    *   im:history
    *   im:read
    *   mpim:history
    *   mpim:read

![image](images/slack-scopes.png)

*   Note your client ID, client secret, and Slack instance name. Client ID and Client secret can be found under 'App Credentials' section.  
    Slack instance name is the workspace name

![image](images/slack-client.png)

*   Install the new app to workspace from 'Install App' under settings. Click 'Allow' for the access permission notification during the installation.

![image](images/slack-install.png)

*   In the last stage of the workshop, you will be analysing the Slack messages with Amazon Quicksight.   Inorder to mock usecase (CI/CD deployment notification messages), post below messages in Slack workspace #general channel

```
Deploy to Staging*** succeeded
Deploy to Production succeeded
Deploy to Staging*** failed
Deploy to Staging*** succeeded
Deploy to Production succeeded
```

## Connect Slack to Appflow and Create the flow

*   Open the Amazon AppFlow console [here](https://console.aws.amazon.com/appflow/)
*   Choose **Create flow** named 'APTWorkshopAppflow'
*   For **Flow details**, enter a name and description for the flow. Leave other fields as default.
*   Choose **Next**.
*   Choose **Slack** from the **Source name** dropdown list.
*   Choose **Connect** to open the **Connect to Slack**  box.
*   Under **Client ID**, enter your Slack client ID.
*   Under **Client secret**, enter your Slack client secret.
*   Under **Workspace**, enter the name of your Slack instance
*   Under **Connection name**, specify a name for your connection.
*   Choose **Continue**.
*   You will be redirected to the Slack login page. When prompted, grant Amazon AppFlow permissions to access your Slack account.

![image](images/slack-connect.png)

*   In the next screen, configure source (Slack) and destination (Amazon S3)
*   Select the S3 bucket from the list.  Note : This bucket was created in the premilinary step.
*   Leave the Flow Trigger option as 'run on demand'

![image](images/appflow-create.png)

*   In the next screen, select 'All map all fields directly' from 'Source field name' dropdown under '**Source to destination field mapping'**

![image](images/appflow-mapping.png)

*   Skip the Filters selection in the next screen and click 'Next'
*   Click 'Create flow' in the 'Review and Create' screen
*   This will create a new flow connecting Slack and S3

![image](images/appflow-created.png)

## Run the Appflow

*   Click on 'Run flow' to run the appflow you just created
*   Once the flow is executed succesfully, data can be viewed from the S3 URL given in the notification window.

![image](images/appflow-result.png)

Note: You can explore the data using Amazon Athena in the next section of the workshop

## Create Athena Table

*   Open the Amazon Athena console [here](https://console.aws.amazon.com/athena/)
*   Click on 'Create Table' and select 'From S3 Bucket'

![image](images/athena-createtable.png)

*   In the next screen, choose a new database name and a table name
*   Type the S3 bucket name that you created in preliminary step

![image](images/athena-s3bucket.png)

*   In Step 2, select JSON as the data format
*   In Step 3, add user, text and ts fields manually (In a real world use case, you can create the table using Amazon Glue to avoid the manual mapping of fields)

![image](images/athena-columns.png)

*   In Step 4, skip the partition setup and click on 'Create Table'

## Query Data in S3

*   In the Athena console, query the new table with below SQL to view the contents of the table

```sql
SELECT * FROM SLACK_DATA;
```

![image](images/athena-queryresults.png)

*   Create a new table called 'SLACK_DATA_ANALYSIS' by running below query

```sql
CREATE TABLE IF NOT EXISTS SLACK_DATA_ANALYSIS
  AS
SELECT
  user,substr(text,11,10) as environment, substr(text,22,9) as status, ts
FROM
  SLACK_DATA
WHERE
  text like '%Deploy%';
```

*   Run a query to view contents of the new table

![image](images/athena-newtable.png)

*   In the last stage of the workshop,  you will analyse the data in table SLACK_DATA_ANALYSIS and create Amazon Quicksight charts.

## Cleanup

At the end of every workshop, you may wish to clean up the stuff you have created so you do not incur additional costs.

# Connecting Jira to Amazon Q

## Basic documentation

- Connecting Jira to Amazon Q: https://docs.aws.amazon.com/amazonq/latest/business-use-dg/jira-connector.html
- CreateDataSource: https://docs.aws.amazon.com/amazonq/latest/api-reference/API_CreateDataSource.html

## Jira

Jira Project:

- Free Tier
- My project URL: https://cloudsteak.atlassian.net

### Create an API token

https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/

1. Log in to https://id.atlassian.com/manage-profile/security/api-tokens
2. Click Create API token
3. From the dialog that appears, enter a Label (`AWS-2-Jira`) for your token and click Create
4. Click Copy to clipboard, then paste the token to your script, or elsewhere to save

## AWS

Amazon Q is available only in several US Region:

- US West (Oregon) - `us-west-2`
- US East (N. Virginia) - `us-east-1`

We use `us-east-1` region

### IAM role for Amazon Q data source connectors

https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/home

### Create Amazon Q Application

https://us-east-1.console.aws.amazon.com/amazonq/home?region=us-east-1#applications

1. Click `Create application` button
   - Application name: `jira-cloudsteak`
   - Create and use a new service role: `QBusiness-Application-jira-cloudsteak`
   - Add some tags
2. Click `Create` button
3. Wait for Role creation
4. On screen `Select retriever` keep the default. Click `Next`
5. On `Connect data sources` screen search for `Jira`
6. Click `+` sign
   - Data source name: jira-ds-cloudsteak
   - Jira Account URL: https://cloudsteak.atlassian.net
   - Authorization: ACL
   - AWS Secrets Manager secret: Create and add a new secret
     - Secret name: `QBusiness-Jira-CloudSteak`
     - Jira ID: (emai)
     - `Password/Token` is the token we created in Jira
   - Identity crawler is on
   - IAM role
     - Create new service role
     - Role name: `QBusiness-DataSource-Jira-CloudSteak`
   - Sync scope: All projects
   - Sync mode: Full sync
   - Sync run schedule
     - Frequency: Hourly (Start minute: 05)
7. `Add data source`
8. Wait for Role creation
9. After some minutes the application is done. You need to go back to applications to see it: https://us-east-1.console.aws.amazon.com/amazonq/home?region=us-east-1#applications

### Run data sync

1. Open application page: https://us-east-1.console.aws.amazon.com/amazonq/home?region=us-east-1#applications
2. Open your application
3. Scroll down to Data sources
4. Select `jira-ds-cloudsteak` data source
5. Click `Sync now`

### Web Experience

**Configure Amazon Q Web Experience with IAM Identity Center as identity provider**

Setting up Amazon Q with IAM Identity Center as identity provider: https://docs.aws.amazon.com/amazonq/latest/business-use-dg/idp-sso.html

1. Open application page: https://us-east-1.console.aws.amazon.com/amazonq/home?region=us-east-1#applications
2. Open your application
3. Scroll down to `Web Experience`
4. Click `Enable Web Experience`
5. then follow the steps in the documentation: https://docs.aws.amazon.com/amazonq/latest/business-use-dg/idp-sso.html

**Use Amazon Q Web Experience**

You can find the document here: https://docs.aws.amazon.com/amazonq/latest/business-use-dg/using-web-experience.html

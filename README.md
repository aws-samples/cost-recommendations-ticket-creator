## Turn Savings Recommendations into Jira Tickets

This solution will walk you through creating an AWS Lambda function to pull AWS Cost Optimization Hub and AWS Trusted Advisor Recommendations and create them as individual tickets in your chosen Jira board. This solution will review the recommendations and create a ticket for each one at the start of the month. 


### Deployment

Step 1 - Get your Jira details

Before we deploy anything, we will need to collect information about your Jira account. This information will be used to create tickets in your chosen project. If you do not have Jira account but want one for this blog you can setup an account using this information here. 
1.	Log in to the your Jira account
2.	To get your Email, use the email you used to login to Jira
3.	To get your Server Name, Copy the start of your url. it should be something like  https://awscostoptimization.atlassian.net
4.	To get your API Token: 
a.	Log in to https://id.atlassian.com/manage-profile/security/api-tokens.
b.	Click Create API token.
c.	From the dialog that appears, enter a memorable and concise Label for your token and click Create.
d.	Click Copy to clipboard, then paste somewhere safe for later in the blog
5.	To get Jira Project information 
a.	Find the project you wish to put your tickets into
b.	Copy the Jira Project Name of the project in figure 1 it is BuildonLiveDemo
c.	Copy the letters in brackets for Jira Project Key like figure 1 COST



Step 2 – Create Secret for API Key

We will now store you API key in SSM Manager Parameter store to keep it safe. Please note that the following resources must all be deployed in the same AWS Region. 

1.	Login to your AWS Console and go to AWS Systems Manager > Parameter Store > Create parameter 

Figure 2: AWS SSM Manager Console View
2.	Use the below configuration:
a.	Call your Parameter Jira_API
b.	Type: SecureString
c.	KMS: My current account (unless your company uses another KMS strategy) 

Figure 3: SSM Parameter Store Example

3.	Paste your Jira API key you got from Step 1 into the 'value' text field, click on Create parameter

Figure 4: SSM Parameter Value Example

Step 3 – Deploy solution to get AWS Trusted Advisor recommendation into your Jira board

Now we will deploy a AWS CloudFormation template that will pull the AWS Trusted Advisor recommendations and turn them into Jira tickets. 
1.	Log in to your AWS Account you wish to deploy this into
2.	[Launch stack button]
3.	Fill out the parameters with the information we collected from Step 1. See example in figure 2

Figure 5: AWS CloudFormation template parameters example
4.	Tick the box next to I acknowledge that AWS CloudFormation might create IAM resources with customised names.
5.	Click Create stack
6.	Wait for the AWS CloudFormation template to finish deploying. It should take about 1 minute 

Step 3 – Test solution 
1.	Click on your AWS CloudFormation Resources Tab and find resource function called jira_ticket_Lambda
2.	Click on it to take you to the AWS Lambda Function
3.	Click on the Test tab. Give the test a name and leave the Event JSON unchanged. Click the Test button on the right

Figure 6: Test function
4.	Once this is finished running log into your Jira account. In your project should see your recommendations as tickets

Figure 7: Trusted Advisor tickets in Jira Example

5.	The AWS Lambda Function will run automatically every * **. The summary of the ticket is a unique identified. The lambda will check if a ticket exists and will not create a duplicate. 
Cleaning up

To avoid incurring future charges, delete the news bot.
1.	Go your AWS CloudFormation Console and find the Jira Stack
2.	Go to your CloudFormation Console. Click on your Stack Delete CloudFormation stack and click Delete



### Useful info


* build code for jira
https://community.atlassian.com/t5/Jira-questions/Jira-Next-Gen-Python-API-Create-Issue-with-Custom-Field/qaq-p/2036396


* build layer
https://towardsdatascience.com/python-packages-in-aws-lambda-made-easy-8fbc78520e30

mkdir python
cd python
cp -r ../v-env/lib64/python3.7/site-packages/* .
cd ..
zip -r jira_layer.zip python
aws lambda publish-layer-version --layer-name jira --zip-file fileb://jira_layer.zip --compatible-runtimes python3.11

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.


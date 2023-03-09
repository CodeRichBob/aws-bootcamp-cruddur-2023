# Week 0 — Billing and Architecture
# About
This documentation is a recap of the activities done and concepts learnt during week 0 of the AWS CLoud Project Bootcamp. 
# Accomplished Tasks
- Watched Week 0 - Live Streamed Video [Here](https://www.youtube.com/watch?v=SG8blanhAOg&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=12)
- Watched Chirag's Week 0 - Spend Considerations [Here](https://www.youtube.com/watch?v=SG8blanhAOg&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=12)
- Watched Ashish's Week 0 - Security Considerations [Here](https://www.youtube.com/watch?v=OVw3RrlP-sI&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=13)
- Recreated Conceptual Diagram in Lucid Charts and on a Napkin
- Recreated Logical Architectual Diagram in Lucid Charts
- Created an Admin User
- Used CloudShell
- Generated AWS Credentials
- Installed AWS CLI
- Created a Billing Alarm
- Created a Budget

# Creating a User
To create a User I followed the following steps as outlined in [this](https://www.iguazio.com/docs/latest-release/cluster-mgmt/deployment/cloud/aws/howto/iam-user-create/) tutorial.
- Log in to the AWS console and search IAM service
- Under the IAM dashboard, go to **Users** , then click on **Add Users** button to create a new user.
- Enter the User details eg name, and **Enable console access**
- **Set permissions** by adding using to either an existing group, or by creating a new group(This is an important step for security reasons as someone is able to set the user's priviledges) 
- Create the user
- Save the User's credentials (**Access key ID** and **Secret Access Key**)

# Install AWS CLI
- To install AWS CLI, you first launch the GitPod environment
- Then set AWS CLI to use partial autoprompt mode to make it easier to debug CLI commands
- Installation commands used are also found [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Update the .gitpod.yml to include the following task.
```
tasks:
  - name: aws-cli
    env:
      AWS_CLI_AUTO_PROMPT: on-partial
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
      
```
The commands are run individually while performing  a manual installation
## Set Env Vars
To configure user credential on the bash terminal, you use the following commands
```
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1
```
For Gitpod to remember the credentials when the workspaces are launched, you run the following commands:
```
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_DEFAULT_REGION=us-east-1

```
To check that the AWS CLI is working and the expected user is correct, I ran the command
```
aws sts get-caller-identity
```
![getcalleridentity](image)

## Budget Via CLI
I had earlier created a zero-spend budget on the AWS Console, however, with the allowance to create one more budget, I decided to try out using the AWS CLI to create one.
- In the project folder I created a new folder called **AWS** and in the folder added another folder called **json**.
- In the json folder, I added two files, **budget.json** and **notifications-with-subscribers.json**
- I Updated the ``` budgets.json ``` file with the following content
```
{
    "BudgetLimit": {
        "Amount": "1",
        "Unit": "USD"
    },
    "BudgetName": "Budget on CLI",
    "BudgetType": "COST",
    "CostFilters": {
        "TagKeyValue": [
            "user:Key$value1",
            "user:Key$value2"
        ]
    },
    "CostTypes": {
        "IncludeCredit": true,
        "IncludeDiscount": true,
        "IncludeOtherSubscription": true,
        "IncludeRecurring": true,
        "IncludeRefund": true,
        "IncludeSubscription": true,
        "IncludeSupport": true,
        "IncludeTax": true,
        "IncludeUpfront": true,
        "UseBlended": false
    },
    "TimePeriod": {
        "Start": 1477958399,
        "End": 3706473600
    },
    "TimeUnit": "MONTHLY"
}
```
- I also updated the ``` notifications-with-subscribers.json ``` file with the following content
```
[
    {
        "Notification": {
            "ComparisonOperator": "GREATER_THAN",
            "NotificationType": "ACTUAL",
            "Threshold": 70,
            "ThresholdType": "PERCENTAGE"
        },
        "Subscribers": [
            {
                "Address": "rgxxxxxxx@gmail.com",
                "SubscriptionType": "EMAIL"
            }
        ]
    }
]
```
- In terminal, run the following commands
```
aws budgets create-budget \
    	--account-id 619023720086 \
    	--budget file://aws/json/budgets.json \
    	--notifications-with-subscribers file://aws/json/notifications-with-subscribers.json
      
```
Budget created on the console
![Budget created on AWS console]()

Budget created using AWS CLI
![budgetoncli]()

## Creating a Billing Alarm via CLI
First, I had to enable billing alerts in the root account, in order to receive alerts
- I created a SNS (Simple Notification Service) topic.-The SNS topic delivers an alert when you are overbilled.
- To create an SNS topic,run the following command
```
aws sns create-topic --name billing-alarm
```
which returns the TopicARN
- I then ran the following command in terminal to create a subscription
```
aws sns subscribe \
    --topic-arn "arn:aws:sns:us-east-1:619023xxxxxx:billing-alarm" \
    --protocol email \
    --notification-endpoint rxxxxxxxxx@gmail.com
```
![arnsubscriptionpending]()

![subscription confirmed]()

- To create a metric alarm, I added configurations to a file named ``` alarm_config.json ``` . In the alarm actions field, I used the previously generated SNS Topic
- In the terminal, run the command 
``` aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json ``` 
![alarm]()

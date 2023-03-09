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

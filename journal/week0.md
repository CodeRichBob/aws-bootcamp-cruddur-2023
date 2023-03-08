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

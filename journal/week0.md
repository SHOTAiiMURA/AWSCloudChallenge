# Week 0 — Billing and Architecture
# Installing AWS CLI
`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install`
make sure path is correct.
# Create an AWS Budget
AWS budget create budget
### Get your AWS Account ID
`aws sts get-caller-identity --query Account --output text`
- Supply your AWS account vID
- Update the join files
- This is anther case with AWS CLI its just much easier to json files to lots of nested json.

`aws budgets create-budget \
    --account-id  339712895985 \
    --budget file://aws/budget.json \
    --notifications-with-subscribers file://aws/budget-notifications-with-subscribers.json`
once create the budget, check AWS management consol to make sure its created.

## Enable Billing
we need to turn on Bliing Alerts to receive alerts.
- in your root account go to the billing page.
- under `billing preference` choose `receive billing alerts`
- save preference.

# Creating a Billing Alarm
### Create SNS topic
- We need an SNS topic beofore we create an alarm.
- The SNS is what will delivery us an alert when get overbilled.
- aws sns create topic
### We'll create a SNS topic
`aws sns create-topic --name billing alarm`
which will return a TopicARN

We'll create a subscription supply the TopicARN and our Email
`aws sns subscribe \
  --topic arn TopicARN \
  --protocol email \
  --notification-endopoint your@email.com`
once its created, check your email and confirm the subscription.
## Create Alarm
- aws cloudwatch put metric alarm
- Create an Alarm AWS CLI
- We need to update the configuration json script with the TopicARN we generate earlier.
- We are just a json file because --metrics is required expression and so its easier to us a JSON file.
`aws cloudwatch put-metric-alarm --cli-input-json file://billing-alarm.json`
make sure path is correct


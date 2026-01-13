# ChatOpsBot
ChatOpsBot is a lightweight DevOps automation assistant that brings AWS operations directly into Slack

Project
ChatOpsBot is a lightweight DevOps automation assistant that brings AWS operations directly into Slack. Traditionally, engineers respond to issues by jumping between the AWS Console, CloudWatch, and various dashboards ,slowing down incident response.

With ChatOpsBot, teams get real-time alerts in Slack, and can trigger fixes like restarting a Lambda or checking logs, without leaving the chat interface.

Services Used 
AWS Chatbot – Connect Slack to AWS
Amazon CloudWatch – Alarms for system health
AWS Lambda / SSM – Run automated fixes
Amazon EventBridge – Route events to Chatbot
AWS IAM – Secure who can run what
AWS CloudFormation – Infrastructure + frontend deployment
Amazon S3 + CloudFront – Host optional dashboard

Architectural Diagram

<img width="828" height="474" alt="Screenshot 2026-01-13 at 9 36 54 AM" src="https://github.com/user-attachments/assets/bd897106-bd6c-467f-b4b7-91f50d36f78d" />

Key outcomes:
Alerts delivered to Slack for immediate visibility
One-click fixes from Slack via Lambda or SSM
IAM controls to ensure safe access
Optional dashboard hosted via S3 + CloudFront

1. Connect Slack with AWS Console.
<img width="1890" height="746" alt="Screenshot 2026-01-13 at 9 44 41 AM" src="https://github.com/user-attachments/assets/66d5be19-9870-46f9-9732-435d077ed49e" />

2. Create an SNS Topic for Slack Alerts

3. Create a SNS Topic. 
<img width="1899" height="725" alt="Screenshot 2026-01-13 at 9 55 00 AM" src="https://github.com/user-attachments/assets/de02ef4c-63d1-4d48-9e9b-c7a2eea6e44f" />

4. Create an email subscription.
<img width="1707" height="752" alt="Screenshot 2026-01-13 at 10 01 05 AM" src="https://github.com/user-attachments/assets/e2c95784-236d-41b2-9a31-04eefbc693b4" />

5. Configure slack channel in Chatbot
<img width="1878" height="644" alt="Screenshot 2026-01-13 at 10 20 52 AM" src="https://github.com/user-attachments/assets/00d171aa-d1e7-4cae-a7d4-5930218d884f" />

6. Test Chatbox Integration
<img width="1290" height="702" alt="Screenshot 2026-01-13 at 10 25 31 AM" src="https://github.com/user-attachments/assets/291f86a1-f36f-4b60-abbc-8bf77e761060" />

Next - I will be creating a real time Cloudwatch Alerts in Slack.
In this step, I will create a CloudWatch alarm that detects high CPU usage on an EC2 instance and sends real-time alerts to  Slack channel using the AWS Chatbot integration.

Step 1. Launch a test EC2.
<img width="1160" height="775" alt="Screenshot 2026-01-13 at 10 35 13 AM" src="https://github.com/user-attachments/assets/af723bbb-5493-47d3-b318-b8c3e7a1f028" />

2. Create a cloudwatch alarm. CloudWatch will monitor CPU usage, and when it goes above a certain threshold, it will trigger an alert.
<img width="1387" height="746" alt="Screenshot 2026-01-13 at 10 44 04 AM" src="https://github.com/user-attachments/assets/003305fc-b39d-48ca-94b0-031046c9270e" />

3. Simulate a CPU spike.

SSH into the EC2 instance to simulate the CPU spike:
Login into EC2 via SSH. 

- I installed stress tool - sudo yum install -y stress
- I run the stress test - stress --cpu 2 --timeout 300
- 




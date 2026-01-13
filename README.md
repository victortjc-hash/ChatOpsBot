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

5. 

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

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
- the test did not hit the 70% threshold so i modified the command to $ stress --cpu 1 --timeout 600
- Results as expected, I received a bot message on Slack as well as Email Notification 

<img width="949" height="741" alt="Screenshot 2026-01-13 at 2 40 56 PM" src="https://github.com/user-attachments/assets/5559cb8d-d86b-48fa-a00f-9b87c89924ba" />
 <img width="1350" height="735" alt="Screenshot 2026-01-13 at 2 40 42 PM" src="https://github.com/user-attachments/assets/148fa23e-d613-45d3-a9d3-6366d83a5244" />

Next, I will automate Operational Tasks from Slack (Lambda Integration).

1. Create a Lambda function with the following script:
<img width="1358" height="780" alt="Screenshot 2026-01-13 at 2 52 52 PM" src="https://github.com/user-attachments/assets/5c0608c8-f428-437b-b412-0cf410df186f" />

2. Add a policy to allow restart of Lambda:
<img width="1551" height="744" alt="Screenshot 2026-01-13 at 2 55 17 PM" src="https://github.com/user-attachments/assets/b03102b9-5429-4f3c-95da-327d58fefdbd" />

3. Invoke lambda function from Slack.
 <img width="1445" height="846" alt="Screenshot 2026-01-13 at 3 20 26 PM" src="https://github.com/user-attachments/assets/b813581c-359c-4682-9202-a6f8a9c86995" />

Next, I will deploy the Frontend Dashboard via CloudFormation
I will demonstrates how to provision the ChatOpsBot frontend dashboard using CloudFormation, following the principles of Operational Excellence in the AWS Well-Architected Framework.
Operational Excellence emphasizes automation, repeatability, and traceability. Using CloudFormation to deploy your frontend infrastructure ensures:

Infrastructure is defined as code (IaC)
Setups are consistent and auditable
Rollbacks and updates are easier and safe

Architecture Context
This CloudFormation template provisions:

An S3 bucket configured for static website hosting
A CloudFront distribution with Origin Access Control (OAC) for secure access
A bucket policy that grants CloudFront access to the bucket only via the OAC
An output URL to access the dashboard globally

Create a yaml file with following script

```
AWSTemplateFormatVersion: '2010-09-09'
Description: ChatOpsBot – Frontend Dashboard (S3 + CloudFront using OAC)

Resources:

  DashboardBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "chatops-frontend-${AWS::AccountId}-${AWS::Region}"
      WebsiteConfiguration:
        IndexDocument: index.html
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true
      OwnershipControls:
        Rules:
          - ObjectOwnership: BucketOwnerEnforced

  CloudFrontOAC:
    Type: AWS::CloudFront::OriginAccessControl
    Properties:
      OriginAccessControlConfig:
        Name: ChatOpsOAC
        Description: Access control for CloudFront to access S3
        SigningBehavior: always
        SigningProtocol: sigv4
        OriginAccessControlOriginType: s3

  DashboardDistribution:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Enabled: true
        DefaultRootObject: index.html
        Origins:
          - DomainName: !GetAtt DashboardBucket.RegionalDomainName
            Id: S3Origin
            S3OriginConfig: {}
            OriginAccessControlId: !Ref CloudFrontOAC
        DefaultCacheBehavior:
          TargetOriginId: S3Origin
          ViewerProtocolPolicy: redirect-to-https
          AllowedMethods: [GET, HEAD]
          CachedMethods: [GET, HEAD]
          Compress: true
          ForwardedValues:
            QueryString: false
        ViewerCertificate:
          CloudFrontDefaultCertificate: true

  BucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      Bucket: !Ref DashboardBucket
      PolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service: cloudfront.amazonaws.com
            Action: s3:GetObject
            Resource: !Sub "${DashboardBucket.Arn}/*"
            Condition:
              StringEquals:
                AWS:SourceArn: !Sub "arn:aws:cloudfront::${AWS::AccountId}:distribution/${DashboardDistribution}"

Outputs:
  DashboardURL:
    Description: CloudFront URL to access the frontend dashboard
    Value: !Sub "<https://$>{DashboardDistribution.DomainName}"
```
Create a stack via cloudformation using  dashboard.yml
<img width="1715" height="660" alt="Screenshot 2026-01-13 at 3 30 00 PM" src="https://github.com/user-attachments/assets/38094472-942b-4adf-9d53-44c34d1717a8" />
Once stack is deployed, 
<img width="1082" height="651" alt="Screenshot 2026-01-13 at 3 31 28 PM" src="https://github.com/user-attachments/assets/f5709328-d1c7-4d9b-934f-e6b0a287acfe" />


---
title: "Resource Cleanup"
date: 2026-04-04
weight: 7
chapter: false
pre: " <b> 4.7. </b> "
---

#### Overview

This section presents the cleanup sequence for the resources created for the **LunchSync** demo environment.

#### Phase 1: Clean Up Backend Runtime and CI/CD

This phase covers the resources used to run the backend, store container images, and connect the deployment pipeline.

##### 1.1 Delete the ECS Service

Go to **ECS Console** → **Clusters**.  
Open the backend cluster for LunchSync.  
In the **Services** tab, select the backend service and choose **Delete service**.

##### 1.2 Deregister Task Definition Revisions

Go to **ECS Console** → **Task definitions**.  
Select the family: `lunchsync-backend`.  
Choose the created revisions and perform **Deregister**.

##### 1.3 Delete the ECR Repository

Go to **Amazon ECR** → **Repositories**.  
Select the repository: `lunchsync-backend`.  
Remove any remaining images if needed, then choose **Delete repository**.

##### 1.4 Delete the IAM Role for GitHub Actions

Go to **IAM Console** → **Roles**.  
Select the role: `lunchsync-cicd-role`.  
Choose **Delete** to remove the CI/CD pipeline role.

##### 1.5 Delete the OIDC Identity Provider

Go to **IAM Console** → **Identity providers**.  
Select the provider: `token.actions.githubusercontent.com`.  
Choose **Delete** if this provider was created only for the workshop.

##### 1.6 Remove GitHub Secrets or Variables

Go to **GitHub Repository** → **Settings** → **Secrets and variables** → **Actions**.  
Remove the workflow configuration entries such as `AWS_REGION`, `AWS_ROLE_ARN`, `ECR_REPOSITORY`, `ECS_CLUSTER`, and `ECS_SERVICE`.

#### Phase 2: Clean Up Frontend and Access Layer

This phase covers the public-facing resources, including CloudFront, S3, Route 53, ACM, and Cognito.

##### 2.1 Delete the CloudFront Distribution

Go to **CloudFront Console** → **Distributions**.  
Select the distribution used for the LunchSync frontend.  
Disable it if required, then choose **Delete**.

##### 2.2 Empty and Delete the S3 Bucket

Go to **S3 Console**.  
Select the bucket that stores the LunchSync frontend build.  
In the **Objects** tab, choose **Empty** to remove all objects, then return to the bucket list and choose **Delete**.

##### 2.3 Delete Route 53 Records and the Hosted Zone

Go to **Route 53 Console** → **Hosted zones**.  
Open the hosted zone used for the demo environment.  
Delete the records pointing to **CloudFront** or **ALB**, then delete the hosted zone if it is no longer needed.

##### 2.4 Delete the ACM Certificate

Go to **AWS Certificate Manager**.  
Select the certificate issued for the LunchSync frontend or backend domain.  
Choose **Delete** to remove the certificate.

##### 2.5 Delete the Cognito User Pool

Go to **Amazon Cognito** → **User pools**.  
Select the user pool created for LunchSync.  
Choose **Delete user pool** to remove the authentication configuration for the demo environment.

#### Phase 3: Clean Up the Data Layer and Security Configuration

This phase applies to the database, the cache layer, and the runtime secrets.

##### 3.1 Delete the Redis Cluster

Go to **ElastiCache Console** → **Redis OSS** or **Valkey/Redis clusters**.  
Select the Redis cluster created for LunchSync.  
Choose **Delete** to remove the cache cluster.

##### 3.2 Delete the RDS Database Instance

Go to **RDS Console** → **Databases**.  
Select the LunchSync database instance.  
Choose **Delete** and select whether to keep or skip the final snapshot according to the retention requirement of the demo environment.

##### 3.3 Delete Secrets Manager Secrets

Go to **AWS Secrets Manager**.  
Select the secrets created for the backend, such as **Cognito**, **database**, **Redis**, or other runtime configuration values.  
Perform **Delete** for each secret that is no longer needed.

#### Phase 4: Clean Up the Network Foundation

The final phase removes the networking resources created for the ALB, backend, and data layer.

##### 4.1 Delete the Application Load Balancer

Go to **EC2 Console** → **Load Balancers**.  
Select the **Application Load Balancer** used by LunchSync.  
Choose **Delete load balancer**.

##### 4.2 Delete the Target Group

Go to **EC2 Console** → **Target Groups**.  
Select the backend target group used by LunchSync.  
Choose **Delete**.

##### 4.3 Delete VPC Endpoints and the NAT Gateway

Go to **VPC Console** → **Endpoints**.  
Delete the endpoints created for the backend private path.  
If the workshop environment includes a **NAT Gateway**, go to **NAT Gateways** and delete the corresponding resource.

##### 4.4 Delete Security Groups

Go to **EC2 Console** → **Security Groups**.  
Delete the security groups created specifically for the **ALB**, **backend**, **RDS**, **Redis**, and endpoints after the dependent resources have been removed.

##### 4.5 Delete the VPC

Go to **VPC Console** → **Your VPCs**.  
Select the VPC created for the LunchSync environment.  
Choose **Delete VPC** to complete the cleanup process.

---
title: "IAM, ECS, and Task Definition Deployment"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.6.2. </b> "
---

#### Overview

This section describes the configuration steps for **IAM**, **Amazon ECS**, and **Task Definition** to deploy the backend on **AWS Fargate**.

#### Deployment Steps

1. Open **IAM > Roles** to prepare roles for ECS.

![IAM step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.1.jpg)

2. Create a new role with trusted entity **AWS service**, select **Elastic Container Service**, and use case **Elastic Container Service Task**.

![IAM step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.2.jpg)

3. Name the role `lunchsync-ecs-task-role` to use as the **task role** for the backend container, without adding permissions.

![IAM step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.3.jpg)
![IAM step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.4.jpg)

4. Create a second role for the **execution role**, and attach the managed policy `AmazonECSTaskExecutionRolePolicy`.

![IAM step 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.7.jpg)

5. Add an inline policy to the execution role so the ECS agent can read secrets injected into the container at runtime.

![IAM step 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.8.jpg)

6. Verify that `lunchsync-ecs-execution-role` includes both `AmazonECSTaskExecutionRolePolicy` and secret access permissions.

![IAM step 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.9.jpg)

7. Open **Amazon ECS** and navigate to **Clusters**.

![ECS step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.1.jpg)

8. Create a new cluster named `lunchsync-cluster` using **Fargate only** infrastructure.

![ECS step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.2.jpg)

9. Optionally configure observability features such as **Container Insights** and **ECS Exec**.

![ECS step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.3.jpg)

10. Complete cluster creation.

![ECS step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.4.jpg)

11. Open `lunchsync-cluster` and confirm it is **Active** with Fargate capacity providers.

![ECS step 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.5.jpg)

12. Navigate to **Task Definitions** and create a new task definition.

![ECS step 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.6.jpg)

13. Set:
   - Family: `lunchsync-backend`  
   - Launch type: **AWS Fargate**  
   - OS/Arch: `Linux/X86_64`  
   - Network mode: `awsvpc`  
   - Task role: `lunchsync-ecs-task-role`  
   - Execution role: `lunchsync-ecs-execution-role`  

![Task Definition step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.1.jpg)

14. Add container `lunchsync-backend`, use image URI from ECR, and expose port `8080`.

![Task Definition step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.2.jpg)

15. Configure environment variables and secret mappings (Cognito, database connection, Redis, `NODE_ENV`, and other runtime configs).

![Task Definition step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.3.jpg)

16. Configure container health check, e.g.:
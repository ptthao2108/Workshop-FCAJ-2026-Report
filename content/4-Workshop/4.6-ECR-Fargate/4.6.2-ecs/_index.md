---
title: "Deploy IAM, ECS, and the task definition"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.6.2. </b> "
---

#### Overview

This section presents the configuration steps for **IAM**, **ECS**, and the **Task Definition** for the backend on **Fargate**.

#### Implementation steps

1. Open **IAM > Roles** to prepare the roles required by ECS.

![IAM step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.1.jpg)

2. Create a new role with **AWS service** as the trusted entity, choose **Elastic Container Service**, and use the **Elastic Container Service Task** use case.

![IAM step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.2.jpg)

3. Name the role `lunchsync-ecs-task-role` so it can be used as the backend **task role**.

![IAM step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.3.jpg)

4. After the role is created, reopen `lunchsync-ecs-task-role` to add secret-reading permissions.

![IAM step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.4.jpg)

5. Create an inline policy that allows `secretsmanager:GetSecretValue` and `DescribeSecret` on the `db`, `redis`, and `cognito` secrets used by the application.

![IAM step 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.5.jpg)

6. Review `lunchsync-ecs-task-role` and confirm that the inline policy for reading secrets is attached.

![IAM step 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.6.jpg)

7. Create the second role for the **execution role** and, during Add permissions, attach the managed policy `AmazonECSTaskExecutionRolePolicy`.

![IAM step 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.7.jpg)

8. Add an inline policy to the execution role as well so the ECS agent can read the secrets it needs to inject into the container at startup.

![IAM step 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.8.jpg)

9. Review `lunchsync-ecs-execution-role` and confirm it has both `AmazonECSTaskExecutionRolePolicy` and the secret access policy.

![IAM step 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/iam.9.jpg)

10. Open **Amazon ECS** and go to **Clusters**.

![ECS step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.1.jpg)

11. Create a new cluster named `lunchsync-cluster` and choose **Fargate only** as the infrastructure model.

![ECS step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.2.jpg)

12. If needed, configure observability features such as **Container Insights** and **ECS Exec**.

![ECS step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.3.jpg)

13. Finish creating the cluster.

![ECS step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.4.jpg)

14. Open `lunchsync-cluster` and confirm that it is **Active** with Fargate capacity providers available.

![ECS step 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.5.jpg)

15. In ECS, switch to **Task definitions** and start creating a new task definition for the backend.

![ECS step 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.6.jpg)

16. Set the family to `lunchsync-backend`, choose **AWS Fargate**, `Linux/X86_64`, `awsvpc`, and attach `lunchsync-ecs-task-role` plus `lunchsync-ecs-execution-role`.

![Task Definition step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.1.jpg)

17. Add container `lunchsync-backend`, use the image URI from ECR, and expose port `8080`.

![Task Definition step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.2.jpg)

18. Add environment variables and secret mappings for Cognito, database connectivity, Redis, `NODE_ENV`, and the other runtime settings used by the app.

![Task Definition step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.3.jpg)

19. Configure a container health check such as `curl -f http://localhost:8080/health || exit 1`.

![Task Definition step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.4.jpg)

20. Create the task definition revision and confirm that `lunchsync-backend` is now **ACTIVE**.

![Task Definition step 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/td.5.jpg)

21. Return to `lunchsync-cluster` and start creating a new **service** using the task definition you just created.

![ECS step 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.7.jpg)

22. Name the service `lunchsync-backend-service`, choose launch type **FARGATE**, and select the `lunchsync-backend` task definition family.

![ECS step 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.8.jpg)

23. Configure networking for the service: use the private subnets in `lunchsync-vpc`, attach `backend-sg`, and keep `Public IP = Off`.

![ECS step 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.9.jpg)

24. Enable load balancing with the existing **Application Load Balancer**, use listener `HTTPS:443`, and map traffic to container `lunchsync-backend:8080`.

![ECS step 10](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.10.jpg)

25. Attach the service to target group `lunchsync-tg-backend`, keep health check path `/health`, and create the service.

![ECS step 11](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.11.jpg)

26. After the service is created, monitor the initial rollout: the service should show `2 Desired tasks`, with deployment still **In progress** for a few minutes.

![ECS step 12](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.12.jpg)

27. If desired, configure **Service Auto Scaling** based on CPU utilization.

![ECS step 13](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.13.jpg)

28. You can add a second auto scaling policy based on memory utilization as well.

![ECS step 14](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.14.jpg)

29. On the final service screen, inspect the **Tasks** tab and confirm tasks are running; if the service still shows states such as `Rollback failed` or unhealthy targets, use **Events/Logs** to troubleshoot container config, secrets, or health checks.

![ECS step 15](/images/4-Workshop/4.6-ECR-Fargate/4.6.2-ecs/ecs.15.jpg)

---
title: "Prepare the private path, ECR, and GitHub Actions"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.6.1. </b> "
---

#### Overview

This section presents the steps to prepare the private path, the **ECR repository**, and **GitHub Actions** for the image build and push workflow.

#### Implementation steps

1. Start by creating security group `vpc-endpoints-sg` inside `lunchsync-vpc`, allow inbound traffic from `backend-sg`, and keep full outbound access for the interface endpoints.

![VPC step 0](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.0.jpg)

2. Open **VPC > Endpoints** and start creating the first interface endpoint.

![VPC step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.1.jpg)

3. Choose a service such as `com.amazonaws.ap-southeast-1.ecr.api`, enable **Private DNS**, attach it to `lunchsync-vpc`, and select the two private subnets.

![VPC step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.2.jpg)

4. Attach `vpc-endpoints-sg`, keep the endpoint policy at `Full access`, and create the endpoint.

![VPC step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.3.jpg)

5. Repeat the pattern for the backend endpoints you need, such as `ecr.dkr`, `secretsmanager`, `logs`, and `cognito-idp`, then review the endpoint list.

![VPC step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/vpc.4.jpg)

6. Move to **Amazon ECR** and start creating the private repository for the backend image.

![ECR step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.1.jpg)

7. Name the repository `lunchsync-backend`.

![ECR step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.2.jpg)

8. Configure repository settings such as tag immutability, default ECR/KMS encryption, and the scan-related options that fit the CI/CD workflow.

![ECR step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.3.jpg)

9. After creation, open `lunchsync-backend` and review its main repository tabs.

![ECR step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.4.jpg)

10. Create the first lifecycle policy rule to clean up old **untagged** images.

![ECR step 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.5.jpg)

11. Add a second rule to keep only the most recent **tagged** images needed for rollback.

![ECR step 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.6.jpg)

12. Review the lifecycle policy after both rules are added and confirm that the policy is saved.

![ECR step 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.7.jpg)

13. Check the lifecycle policy tab one more time to confirm the repository now cleans up images as intended.

![ECR step 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.8.jpg)

14. From the repository list, open **View push commands** to capture the exact login, build, tag, and push URI.

![ECR step 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.9.jpg)

15. Save the full ECR push commands because the same values will be used in the workflow or during manual validation.

![ECR step 10](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.10.jpg)

16. Once ECR is ready, switch to **IAM > Identity providers** and add the GitHub OIDC provider.

![Git step 1](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.1.jpg)

17. Choose **OpenID Connect**, enter `https://token.actions.githubusercontent.com`, and use `sts.amazonaws.com` as the audience.

![Git step 2](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.2.jpg)

18. Review the provider list and confirm that the GitHub provider now exists.

![Git step 3](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.3.jpg)

19. Create a new role with **Web identity** as the trusted entity so GitHub Actions can assume it.

![Git step 4](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.4.jpg)

20. Scope the trust relationship to GitHub organization `pttha02108`, repository `lunchsync`, and branch `main`.

![Git step 5](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.5.jpg)

21. Name the role `lunchsync-cicd-role` and create it.

![Git step 6](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.6.jpg)

22. Open the new role and create an inline policy for the pipeline.

![Git step 7](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.7.jpg)

23. In the JSON policy, allow the pipeline to log in to ECR, push images, register new task definitions, update the ECS service, and access the related AWS resources it needs.

![Git step 8](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.8.jpg)

24. Name the policy, for example `lunchsync-cicd-policy`, and save it.

![Git step 9](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.9.jpg)

25. Review `lunchsync-cicd-role` and confirm the policy is attached correctly.

![Git step 10](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.10.jpg)

26. Return to the GitHub repository and open **Settings > Secrets and variables > Actions**.

![Git step 11](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.11.jpg)

27. Add the repository secrets needed by the workflow, such as `AWS_REGION`, `AWS_ROLE_ARN`, `ECR_REPOSITORY`, `ECS_CLUSTER`, and `ECS_SERVICE`.

![Git step 12](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.12.jpg)

28. Review `.github/workflows` and confirm files such as `ci.yml` and `cd.yml` are present.

![Git step 13](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/git.13.jpg)

29. Run a manual `docker login` to ECR and a `docker build` for the backend image from `LunchSync.Api/Dockerfile`.

![ECR step 11](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.11.jpg)

30. Tag the image with the correct `lunchsync-backend` repository URI and push it to ECR.

![ECR step 12](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.12.jpg)

31. Return to ECR and confirm that the new image and tag now appear in the repository.

![ECR step 13](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.13.jpg)

32. Open the image details page and capture the full digest and image URI for the task definition step.

![ECR step 14](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.14.jpg)

33. Review the image list, tags, and push timestamps one last time to confirm the repository is ready for ECS deployment.

![ECR step 15](/images/4-Workshop/4.6-ECR-Fargate/4.6.1-cicdpipeline-ecr/ecr.15.jpg)

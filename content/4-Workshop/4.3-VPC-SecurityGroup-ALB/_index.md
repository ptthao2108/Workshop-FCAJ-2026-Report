---
title: "VPC, Security Group, and ALB"
date: 2026-04-04
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

#### Overview

This section presents the configuration steps for **VPC**, **Target Group**, **ALB**, and **Security Group** in the network layer.

#### Implementation steps

1. Open the **VPC console**, choose **Create VPC**, and use the **VPC and more** template so AWS generates the base networking components.

![VPC step 1](vpc.1.jpg)

2. Name the VPC `lunchsync-vpc`, use CIDR `10.0.0.0/16`, and create 2 AZs, 2 public subnets, and 2 private subnets.

![VPC step 2](vpc.2.jpg)

3. Set **NAT gateways** to `None`, enable the **S3 Gateway Endpoint**, and keep **Enable DNS hostnames / DNS resolution** turned on so private subnets can still reach S3.

![VPC step 3](vpc.3.jpg)

4. Create the VPC and verify that AWS provisioned the public/private subnets, route tables, internet gateway, and S3 endpoint.

![VPC step 4](vpc.4.jpg)

5. Move to **EC2 > Target Groups** and begin creating the backend target group.

![Target Group step 1](tg.1.jpg)

6. Select **Target type = IP addresses**, name it `lunchsync-tg-backend`, and use protocol `HTTP` on port `8080`.

![Target Group step 2](tg.2.jpg)

7. Attach the target group to `lunchsync-vpc`, keep **Protocol version = HTTP1**, and set the health check path to `/health`.

![Target Group step 3](tg.3.jpg)

8. In **Register targets**, note that ECS will register task IPs later, so you can continue without adding manual IPs now.

![Target Group step 4](tg.4.jpg)

9. Review the final target group settings: protocol `HTTP:8080`, health check on `traffic-port`, path `/health`, and success code `200`.

![Target Group step 5](tg.5.jpg)

10. After creation, confirm that `lunchsync-tg-backend` exists and currently has no registered targets.

![Target Group step 6](tg.6.jpg)

11. Open **EC2 > Load Balancers** and start creating a new load balancer.

![ALB step 1](alb.1.jpg)

12. Choose **Application Load Balancer**.

![ALB step 2](alb.2.jpg)

13. Name it `lunchsyn-alb`, choose `Internet-facing`, and keep `IPv4`.

![ALB step 3](alb.3.jpg)

14. Select `lunchsync-vpc` and attach the ALB to the two public subnets `lunchsync-subnet-public1` and `lunchsync-subnet-public2`.

![ALB step 4](alb.4.jpg)

15. Create listener `HTTP:80` with the action **Redirect to HTTPS** and status code `301`.

![ALB step 5](alb.5.jpg)

16. Create listener `HTTPS:443` and forward its default action to `lunchsync-tg-backend`.

![ALB step 6](alb.6.jpg)

17. In **Secure listener settings**, select the ACM certificate for `lunchsync.space` and keep the recommended security policy.

![ALB step 7](alb.7.jpg)

18. Review the integration section as well, especially if you plan to limit ALB access so traffic only comes through CloudFront later.

![ALB step 8](alb.8.jpg)

19. Review the summary page: 2 public subnets, `80 -> 443`, and `443 -> lunchsync-tg-backend`, then create the load balancer.

![ALB step 9](alb.9.jpg)

20. After the ALB is created, inspect **Listeners and rules** to confirm that `HTTP:80` redirects and `HTTPS:443` forwards correctly.

![ALB step 10](alb.10.jpg)

21. Open the **Security** tab, verify the attached security group, and keep the ALB DNS name for the CloudFront section.

![ALB step 11](alb.11.jpg)

22. Confirm that the ALB security group is now scoped to CloudFront or the relevant prefix list rather than being broadly open to the internet.

![ALB step 12](alb.12.jpg)

23. Open **Security Groups** to review the groups in the VPC and prepare the backend group.

![Security Group step 1](sg.1.jpg)

24. Create `backend-sg` in `lunchsync-vpc`, allow inbound `Custom TCP 8080` from the ALB/CloudFront security group, and keep outbound `All traffic`.

![Security Group step 2](sg.2.jpg)

25. Review `redis-sg` and make sure it only allows `TCP 6379` from `backend-sg`.

![Security Group step 3](sg.3.jpg)

26. Review `rds-sg` and make sure it only allows `PostgreSQL 5432` from `backend-sg`.

![Security Group step 4](sg.4.jpg)

27. The final overview screenshot is used to quickly verify the workshop security groups as a whole: the ALB/CloudFront group, `backend-sg`, `redis-sg`, `rds-sg`, and the default groups inside the VPC.

![Security Group summary](Screenshot%202026-04-03%20001755.jpg)

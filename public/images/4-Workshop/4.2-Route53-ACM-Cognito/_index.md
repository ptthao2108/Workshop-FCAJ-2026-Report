---
title: "Route53, ACM, and Cognito"
date: 2026-04-04
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

#### Overview

This section presents the **Route 53**, **ACM**, and **Cognito** configuration steps for the LunchSync access and authentication layer.

#### Implementation steps

1. Open **Amazon Route 53**, go to **Hosted zones**, and start creating a new hosted zone for the workshop domain.

![Route53 step 1](4.2.1-Route53-ACM/route53.1.jpg)

2. In **Create hosted zone**, enter `lunchsync.space`, choose **Public hosted zone**, and create the zone.

![Route53 step 2](4.2.1-Route53-ACM/route53.2.jpg)

3. After the zone is created, review the default **NS** and **SOA** records for `lunchsync.space`.

![Route53 step 3](4.2.1-Route53-ACM/route53.3.jpg)

4. Copy the 4 nameservers from the **NS** record and update them at your domain registrar so Route 53 becomes the DNS authority for `lunchsync.space`.

![Route53 step 4](4.2.1-Route53-ACM/route53.4.jpg)

5. Move to **AWS Certificate Manager (ACM)** in the region used by the backend and start a public certificate request.

![ACM step 1](4.2.1-Route53-ACM/acm.1.jpg)

6. Choose **Request a public certificate**, enter `lunchsync.space` and `*.lunchsync.space`, and use **DNS validation**.

![ACM step 2](4.2.1-Route53-ACM/acm.2.jpg)

7. Keep the default key algorithm such as `RSA 2048`, review the request, and submit the certificate request.

![ACM step 3](4.2.1-Route53-ACM/acm.3.jpg)

8. Open the new certificate details page and inspect the **CNAME** records used for DNS validation in Route 53.

![ACM step 4](4.2.1-Route53-ACM/acm.4.jpg)

9. If ACM shows **Create records in Route 53**, use it to create the missing validation records directly from ACM.

![ACM step 5](4.2.1-Route53-ACM/acm.5.jpg)

10. Wait until the certificate in the application region changes to **Issued**, then keep its ARN for the ALB step.

![ACM step 6](4.2.1-Route53-ACM/acm.6.jpg)

11. Repeat the certificate request flow in **US East (N. Virginia)** so CloudFront has its own certificate, then confirm that this certificate also reaches **Issued**.

![ACM step 7](4.2.1-Route53-ACM/acm.7.jpg)

12. Open **Amazon Cognito** and choose **Create user pool** for LunchSync.

![Cognito step 1](4.2.2-Cognito/co.1.jpg)

13. Choose **Traditional web application**, name the app `Lunchsync-App`, use `Email` and `Username` for sign-in, enable **Self-registration**, and keep the required sign-up attributes such as `email` and `name`.

![Cognito step 2](4.2.2-Cognito/co.2.jpg)

14. Set the **Return URL** to `https://lunchsync.space/`, then finish the quick setup so Cognito creates both the user pool and app client.

![Cognito step 3](4.2.2-Cognito/co.3.jpg)

15. Reopen the new pool and confirm that `Lunchsync-App` is attached to `Lunchsync - user pool`.

![Cognito step 4](4.2.2-Cognito/co.4.jpg)

16. Open **App client** to capture the **Client ID**, inspect the **Client secret**, and review token settings.

![Cognito step 5](4.2.2-Cognito/co.5.jpg)

17. Open the **Sign-in** tab of the user pool and review sign-in options, MFA, and device-tracking settings.

![Cognito step 6](4.2.2-Cognito/co.6.jpg)

18. Edit **App client information** if needed and make sure the client allows the required flows such as `ALLOW_USER_AUTH` and `ALLOW_REFRESH_TOKEN_AUTH`.

![Cognito step 7](4.2.2-Cognito/co.7.jpg)

19. Move to **AWS Secrets Manager**, choose **Other type of secret**, and store Cognito values such as `cognito_client_id`, `cognito_user_pool_id`, and `cognito_client_secret`.

![Cognito step 8](4.2.2-Cognito/co.8.jpg)

20. Name the secret `lunchsync/cognito` so the backend can reference it consistently at runtime.

![Cognito step 9](4.2.2-Cognito/co.9.jpg)

21. Finish creating the secret and confirm that `lunchsync/cognito` appears in the Secrets Manager list together with the other application secrets.

![Cognito step 10](4.2.2-Cognito/co.10.jpg)

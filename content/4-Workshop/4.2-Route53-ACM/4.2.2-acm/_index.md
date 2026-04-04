---
title: "Request and validate ACM certificate"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2.2. </b> "
---

1. Open **AWS Certificate Manager (ACM)** and choose to request a public certificate.

![ACM step 1](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.1.jpg)

1. Enter the domain names that should be protected by the certificate.

![ACM step 2](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.2.jpg)

3. Choose **DNS validation** so Route53 can be used to complete the validation flow.

![ACM step 3](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.3.jpg)

4. Review the request details and submit the certificate request.

![ACM step 4](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.4.jpg)

5. Open the generated validation records and create or confirm the CNAME records in Route53.

![ACM step 5](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.5.jpg)

6. Wait until the validation is completed and the certificate status changes to **Issued**.

![ACM step 6](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.6.jpg)

7. Verify the final certificate details before using it with the load balancer or CloudFront.

![ACM step 7](/images/4-Workshop/4.2-Route53-ACM/4.2.2-acm/acm.7.jpg)

---
title: "S3 and CloudFront"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

#### Overview

This section describes the configuration steps for **Amazon S3**, **Origin Access Control (OAC)**, and **CloudFront** to serve the static frontend of the system.

#### Deployment Steps

1. Open **Amazon S3** and create a new bucket for the LunchSync frontend.

![S3 step 1](/images/4-Workshop/4.4-S3-CloudFront/s3.1.jpg)

2. Set the bucket prefix to `lunchsync-project`, choose region `ap-southeast-1`, allow S3 to generate the full name, and keep the bucket private.

![S3 step 2](/images/4-Workshop/4.4-S3-CloudFront/s3.2.jpg)

3. Keep `ACLs disabled`, enable **Block all public access**, do not enable versioning, and continue with a private bucket.

![S3 step 3](/images/4-Workshop/4.4-S3-CloudFront/s3.3.jpg)

4. Use default encryption `SSE-S3`, create the bucket, then upload frontend content into folders such as `frontend/` and `media/`.

![S3 step 4](/images/4-Workshop/4.4-S3-CloudFront/s3.4.jpg)

5. After attaching the bucket to CloudFront, check the **bucket policy** to ensure only `cloudfront.amazonaws.com` from your distribution can read objects.

![S3 step 5](/images/4-Workshop/4.4-S3-CloudFront/s3.5.jpg)

6. Go to **CloudFront > Origin access** and create a new **Origin Access Control (OAC)** for the S3 bucket.

![OAC step 1](/images/4-Workshop/4.4-S3-CloudFront/oac.1.jpg)

7. Name the OAC `lunchsync-oac`, add a clear description, and keep **Sign requests** enabled.

![OAC step 2](/images/4-Workshop/4.4-S3-CloudFront/oac.2.jpg)

8. Verify that `lunchsync-oac` has been created successfully before proceeding.

![OAC step 3](/images/4-Workshop/4.4-S3-CloudFront/oac.3.jpg)

9. Go to **CloudFront > Distributions** and start creating a new distribution.

![CloudFront step 1](/images/4-Workshop/4.4-S3-CloudFront/cf.1.jpg)

10. Choose the distribution setup plan and continue the wizard.

![CloudFront step 2](/images/4-Workshop/4.4-S3-CloudFront/cf.2.jpg)

11. Name the distribution `S3-Frontend`, select **Single website or app**, and skip Route 53 domain setup in the initial wizard.

![CloudFront step 3](/images/4-Workshop/4.4-S3-CloudFront/cf.3.jpg)

12. In **Specify origin**, choose **Amazon S3** as the origin type.

![CloudFront step 4](/images/4-Workshop/4.4-S3-CloudFront/cf.4.jpg)

13. Select the project S3 bucket as origin, set **Origin path = /frontend**, and allow CloudFront to access the private bucket.

![CloudFront step 5](/images/4-Workshop/4.4-S3-CloudFront/cf.5.jpg)

14. Keep AWS recommended origin and cache settings for static S3 content.

![CloudFront step 6](/images/4-Workshop/4.4-S3-CloudFront/cf.6.jpg)

15. In **Enable security**, keep default CloudFront/WAF protections.

![CloudFront step 7](/images/4-Workshop/4.4-S3-CloudFront/cf.7.jpg)

16. Review configuration: S3 origin, `Origin path = /frontend`, then create the distribution.

![CloudFront step 8](/images/4-Workshop/4.4-S3-CloudFront/cf.8.jpg)

17. After creation, open **Origins** tab to verify correct origin path and OAC.

![CloudFront step 9](/images/4-Workshop/4.4-S3-CloudFront/cf.9.jpg)

18. If needed, edit origin to attach `lunchsync-oac` and copy the generated policy into the S3 bucket policy.

![CloudFront step 10](/images/4-Workshop/4.4-S3-CloudFront/cf.10.jpg)

19. Open **Behaviors** tab to configure default behavior.

![CloudFront step 11](/images/4-Workshop/4.4-S3-CloudFront/cf.11.jpg)

20. Set **Viewer protocol policy = Redirect HTTP to HTTPS**.

![CloudFront step 12](/images/4-Workshop/4.4-S3-CloudFront/cf.12.jpg)

21. Keep cache policy `CachingOptimized` and origin request policy `CORS-S3Origin`.

![CloudFront step 13](/images/4-Workshop/4.4-S3-CloudFront/cf.13.jpg)

22. Open **Error pages** and configure custom error response for SPA.

![CloudFront step 14](/images/4-Workshop/4.4-S3-CloudFront/cf.14.jpg)

23. Create a rule: when origin returns `403`, respond with `/index.html` and status `200`.

![CloudFront step 15](/images/4-Workshop/4.4-S3-CloudFront/cf.15.jpg)

24. Verify SPA fallback rule is saved.

![CloudFront step 16](/images/4-Workshop/4.4-S3-CloudFront/cf.16.jpg)

25. Go back to **Origins** and create a second origin for the backend **ALB**.

![CloudFront step 17](/images/4-Workshop/4.4-S3-CloudFront/cf.17.jpg)

26. Enter ALB DNS name, use **HTTPS only** for origin communication.

![CloudFront step 18](/images/4-Workshop/4.4-S3-CloudFront/cf.18.jpg)

27. Create a third origin for media using the same S3 bucket with **Origin path = /media** and attach OAC.

![CloudFront step 19](/images/4-Workshop/4.4-S3-CloudFront/cf.19.jpg)

28. Verify there are now 3 origins: `frontend`, `ALB backend`, and `media`.

![CloudFront step 20](/images/4-Workshop/4.4-S3-CloudFront/cf.20.jpg)

29. Create behavior for `media/*`, route to media origin, enforce HTTPS.

![CloudFront step 21](/images/4-Workshop/4.4-S3-CloudFront/cf.21.jpg)

30. Create behavior for `api/*`, route to ALB, set HTTPS only, disable caching, use `AllViewer` policy.

31. Verify final behavior list.

![CloudFront step 23](/images/4-Workshop/4.4-S3-CloudFront/cf.24.jpg)

32. Go to **Route 53**, open hosted zone `lunchsync.space`.

![CloudFront step 26](/images/4-Workshop/4.4-S3-CloudFront/cf.26.jpg)

33. Create an alias record at root domain pointing to CloudFront distribution.

![CloudFront step 27](/images/4-Workshop/4.4-S3-CloudFront/cf.27.jpg)

34. Verify the alias record appears.

![CloudFront step 28](/images/4-Workshop/4.4-S3-CloudFront/cf.28.jpg)

35. Return to **CloudFront settings**, add **Alternate domain (CNAME)** `lunchsync.space` and select ACM certificate in `us-east-1`.

![CloudFront step 29](/images/4-Workshop/4.4-S3-CloudFront/cf.29.jpg)
![CloudFront step 30](/images/4-Workshop/4.4-S3-CloudFront/cf.30.jpg)

36. Set **Default root object = index.html** and enable HTTP versions (`HTTP/2`, `HTTP/3`).

![CloudFront step 31](/images/4-Workshop/4.4-S3-CloudFront/cf.31.jpg)

37. Save settings and verify domain, certificate, and root object.

![CloudFront step 32](/images/4-Workshop/4.4-S3-CloudFront/cf.32.jpg)

38. Open `https://lunchsync.space` to verify frontend is served via CloudFront.

![CloudFront step 33](/images/4-Workshop/4.4-S3-CloudFront/cf.33.jpg)

39. Test a media asset like `https://lunchsync.space/media/lunchsync.png` to confirm `media/*` behavior works correctly.

![CloudFront step 34](/images/4-Workshop/4.4-S3-CloudFront/cf.34.jpg)
---
title: "S3 and CloudFront"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

#### Overview

This section describes the steps to configure **S3**, **Origin Access Control**, and **CloudFront** for the system’s static frontend.

#### Deployment Steps

1. Open **Amazon S3** and start creating a new bucket for the LunchSync frontend.

![S3 step 1](/images/4-Workshop/4.4-S3-CloudFront/s3.1.jpg)

2. Set the bucket prefix to `lunchsync-project`, choose region `ap-southeast-1`, let S3 generate the full name based on account/region, and keep the bucket private.

![S3 step 2](/images/4-Workshop/4.4-S3-CloudFront/s3.2.jpg)

3. Keep `ACLs disabled`, enable **Block all public access**, do not enable versioning, and continue with a private bucket.

![S3 step 3](/images/4-Workshop/4.4-S3-CloudFront/s3.3.jpg)

4. Keep the default encryption `SSE-S3`, create the bucket, then upload frontend content into folders such as `frontend/` and `media/`.

![S3 step 4](/images/4-Workshop/4.4-S3-CloudFront/s3.4.jpg)

5. After attaching the bucket to CloudFront, check the **bucket policy** to ensure only the `cloudfront.amazonaws.com` service from the distribution can read objects.

![S3 step 5](/images/4-Workshop/4.4-S3-CloudFront/s3.5.jpg)

6. Go to **CloudFront > Origin access** and start creating a new **Origin Access Control (OAC)** for the S3 bucket.

![OAC step 1](/images/4-Workshop/4.4-S3-CloudFront/oac.1.jpg)

7. Name the OAC `lunchsync-oac`, provide a clear description for the LunchSync bucket, and keep **Sign requests** enabled.

![OAC step 2](/images/4-Workshop/4.4-S3-CloudFront/oac.2.jpg)

8. Verify the OAC list to confirm `lunchsync-oac` has been created before proceeding to distribution setup.

![OAC step 3](/images/4-Workshop/4.4-S3-CloudFront/oac.3.jpg)

9. Open **CloudFront > Distributions** and start creating a new distribution.

![CloudFront step 1](/images/4-Workshop/4.4-S3-CloudFront/cf.1.jpg)

10. Choose the distribution setup plan and continue with the wizard.

![CloudFront step 2](/images/4-Workshop/4.4-S3-CloudFront/cf.2.jpg)

11. Name the distribution `S3-Frontend`, select **Single website or app**, and skip attaching a Route 53 domain in the initial wizard.

![CloudFront step 3](/images/4-Workshop/4.4-S3-CloudFront/cf.3.jpg)

12. In **Specify origin**, select **Amazon S3** as the origin type.

![CloudFront step 4](/images/4-Workshop/4.4-S3-CloudFront/cf.4.jpg)

13. Select the project’s S3 bucket as the origin, set **Origin path** to `/frontend`, and enable access to the private bucket.

![CloudFront step 5](/images/4-Workshop/4.4-S3-CloudFront/cf.5.jpg)

14. Keep AWS-recommended origin/cache settings for static content from S3.

![CloudFront step 6](/images/4-Workshop/4.4-S3-CloudFront/cf.6.jpg)

15. In **Enable security**, keep the default CloudFront/WAF protections in the wizard.

![CloudFront step 7](/images/4-Workshop/4.4-S3-CloudFront/cf.7.jpg)

16. Review the configuration: S3 origin pointing to the LunchSync bucket, `Origin path = /frontend`, then create the distribution.

![CloudFront step 8](/images/4-Workshop/4.4-S3-CloudFront/cf.8.jpg)

17. After creation, open the **Origins** tab to confirm the S3 origin is configured correctly with the right origin path and OAC.

![CloudFront step 9](/images/4-Workshop/4.4-S3-CloudFront/cf.9.jpg)

18. If needed, go to **Edit origin** to reattach `lunchsync-oac` and copy the sample policy into the S3 bucket policy.

![CloudFront step 10](/images/4-Workshop/4.4-S3-CloudFront/cf.10.jpg)

19. Open the **Behaviors** tab and edit the default behavior for the frontend.

![CloudFront step 11](/images/4-Workshop/4.4-S3-CloudFront/cf.11.jpg)

20. Set **Viewer protocol policy = Redirect HTTP to HTTPS**.

![CloudFront step 12](/images/4-Workshop/4.4-S3-CloudFront/cf.12.jpg)

21. Use appropriate cache policy like `CachingOptimized` and origin request policy such as `CORS-S3Origin`.

![CloudFront step 13](/images/4-Workshop/4.4-S3-CloudFront/cf.13.jpg)

22. Open the **Error pages** tab and start creating custom error responses for the SPA.

![CloudFront step 14](/images/4-Workshop/4.4-S3-CloudFront/cf.14.jpg)

23. Create a rule so when the origin returns `403`, CloudFront responds with `/index.html` and status `200` for frontend routes.

![CloudFront step 15](/images/4-Workshop/4.4-S3-CloudFront/cf.15.jpg)

24. Verify the error pages list to confirm the SPA fallback rule is saved.

![CloudFront step 16](/images/4-Workshop/4.4-S3-CloudFront/cf.16.jpg)

25. Go back to **Origins** and add a second origin: the backend **ALB**.

![CloudFront step 17](/images/4-Workshop/4.4-S3-CloudFront/cf.17.jpg)

26. For the ALB origin, input the ALB DNS name and use **HTTPS only** for communication.

![CloudFront step 18](/images/4-Workshop/4.4-S3-CloudFront/cf.18.jpg)

27. Add a third origin for media from S3, reuse the same bucket but set **Origin path = /media** and attach `lunchsync-oac`.

![CloudFront step 19](/images/4-Workshop/4.4-S3-CloudFront/cf.19.jpg)

28. Verify the **Origins** tab now includes 3 origins: `frontend`, `ALB backend`, and `media`.

![CloudFront step 20](/images/4-Workshop/4.4-S3-CloudFront/cf.20.jpg)

29. Create a new behavior for `media/*`, route to the media origin, and enforce HTTPS.

![CloudFront step 21](/images/4-Workshop/4.4-S3-CloudFront/cf.21.jpg)

30. For the `media/*` behavior:

![CloudFront step 22](/images/4-Workshop/4.4-S3-CloudFront/cf.22.jpg)
![CloudFront step 23](/images/4-Workshop/4.4-S3-CloudFront/cf.23.jpg)

31. Create another behavior for `api/*`, route to the ALB origin, enforce HTTPS only, disable caching, and use `AllViewer` origin request policy.

32. Review the final behaviors list.

![CloudFront step 24](/images/4-Workshop/4.4-S3-CloudFront/cf.24.jpg)

33. Go to **Route 53**, open hosted zone `lunchsync.space`, and prepare to create an alias record for CloudFront.

![CloudFront step 26](/images/4-Workshop/4.4-S3-CloudFront/cf.26.jpg)

34. Create an alias record at the root domain `lunchsync.space` pointing to the CloudFront distribution.

![CloudFront step 27](/images/4-Workshop/4.4-S3-CloudFront/cf.27.jpg)

35. Verify the hosted zone to confirm the alias record to CloudFront exists.

![CloudFront step 28](/images/4-Workshop/4.4-S3-CloudFront/cf.28.jpg)

36. Go back to **CloudFront > Edit settings**, add **Alternate domain name (CNAME)** as `lunchsync.space`, and select the ACM certificate in `us-east-1`.

![CloudFront step 29](/images/4-Workshop/4.4-S3-CloudFront/cf.29.jpg)
![CloudFront step 30](/images/4-Workshop/4.4-S3-CloudFront/cf.30.jpg)

37. In the same settings, set **Default root object = index.html** and enable HTTP versions like `HTTP/2` and `HTTP/3`.

![CloudFront step 31](/images/4-Workshop/4.4-S3-CloudFront/cf.31.jpg)

38. Save settings and verify the distribution overview to confirm alias `lunchsync.space`, certificate, and `index.html` are applied.

![CloudFront step 32](/images/4-Workshop/4.4-S3-CloudFront/cf.32.jpg)

39. Open `https://lunchsync.space` to verify the frontend is served via CloudFront with the custom domain.

![CloudFront step 33](/images/4-Workshop/4.4-S3-CloudFront/cf.33.jpg)

40. Test an asset such as `https://lunchsync.space/media/lunchsync.png` to confirm the `media/*` behavior works correctly.

![CloudFront step 34](/images/4-Workshop/4.4-S3-CloudFront/cf.34.jpg)
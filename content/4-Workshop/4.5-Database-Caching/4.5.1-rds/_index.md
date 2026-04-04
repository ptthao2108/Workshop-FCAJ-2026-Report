---
title: "Provision RDS"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.5.1. </b> "
---

#### Overview

This section presents the steps to provision the **RDS instance** and prepare the basic backend connectivity settings.

#### Implementation steps

1. Before creating the database, open **RDS > Subnet groups** to prepare a DB subnet group for the private subnets.

![RDS step 0.1](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.1.jpg)

2. Create the subnet group `lunchsync-rds-subnet`, describe it clearly for LunchSync RDS, and choose `lunchsync-vpc`.

![RDS step 0.2](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.2.jpg)

3. Select the two private subnets in `ap-southeast-1a` and `ap-southeast-1b`, then add them to the subnet group.

![RDS step 0.3](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.3.jpg)

4. Review the subnet group list and confirm that `lunchsync-rds-subnet` is in **Complete** status.

![RDS step 0.4](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.0.4.jpg)

5. Open **Databases** and choose **Create database** to start provisioning PostgreSQL.

![RDS step 1](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.1.jpg)

6. Choose **PostgreSQL**, use **Standard create / Full configuration**, pick a `Dev/Test` style template, and keep the deployment **Single-AZ**.

![RDS step 2](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.2.jpg)

![RDS step 3](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.3.jpg)

7. Set `DB instance identifier = lunchsync-db`, use `lunchsync` as the master username, and let **AWS Secrets Manager** manage the credentials.

![RDS step 4](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.4.jpg)

![RDS step 5](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.5.jpg)

8. Choose instance class `db.t3.micro`, storage type `gp3`, then configure connectivity with `lunchsync-vpc`, subnet group `lunchsync-rds-subnet`, `Public access = No`, and security group `rds-sg`.

![RDS step 6](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.6.jpg)

![RDS step 7](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.7.jpg)

9. Review the additional configuration, backup, and maintenance settings, then submit the database request.

![RDS step 8](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.8.jpg)

10. When `lunchsync-db` reaches **Available**, record the `:5432` endpoint, the secret ARN created by RDS, and confirm that the database is not publicly accessible.

![RDS step 9](/images/4-Workshop/4.5-Database-Caching/4.5.1-rds/rds.9.jpg)

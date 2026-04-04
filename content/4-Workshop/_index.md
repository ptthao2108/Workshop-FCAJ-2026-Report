---
title: "Workshop"
date: 2026-03-20
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# LUNCHSYNC PLATFORM - AWS Infrastructure Workshop

This document describes the step-by-step process for deploying the **LunchSync** system on AWS, following the project’s real-world architecture. The objective is to set up all core infrastructure components, including DNS, SSL/TLS certificates, VPC networking, load balancing, user authentication, static content delivery, relational databases, and caching.

The workshop series is organized in the correct deployment sequence: starting with domain and certificate preparation, followed by networking and security setup, authentication configuration, and finally completing the data layer and performance optimization to ensure the system is ready for cloud operation.

#### Contents

1. [Workshop Overview](4.1-Workshop-overview/)
2. [Route53, ACM, and Cognito](4.2-Route53-ACM-Cognito/)
3. [VPC, Security Group, and ALB](4.3-VPC-SecurityGroup-ALB/)
4. [S3 and CloudFront](4.4-S3-CloudFront/)
5. [Data Storage](4.5-Database-Caching/)
6. [Backend Operations](4.6-ECR-Fargate/)
7. [Resource Cleanup](4.7-Cleanup/)
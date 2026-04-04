---
title : "Introduction"
date : 2026-03-20 
weight : 1 
chapter : false
pre : " <b> 4.1. </b> "
---

#### Overview

This workshop explains how to deploy the **LunchSync** system on AWS based on the actual project architecture. The solution is organized as a multi-layer cloud setup that includes domain routing, TLS certificates, private networking, load balancing, user authentication, static content delivery, a relational database, and a caching layer.

#### System components

The LunchSync system is deployed on AWS with the following main components:

- **Route53 and ACM** to manage DNS and provision SSL/TLS certificates for the application domain
- **VPC, Security Group, Target Group, and ALB** to build the network layer, control access, and route traffic to backend services
- **Amazon Cognito** to support user authentication and secure sign-in flows
- **Amazon S3 and CloudFront** to host and distribute the static frontend efficiently
- **Amazon RDS** to provide the relational database layer
- **Redis** to add caching and improve application response performance

#### Workshop Architecture

![Workshop architecture](/images/proposal/lunchsync.png)

#### Workshop overview

In this workshop, you will deploy a multi-stage LunchSync environment on AWS by building each infrastructure layer step by step. It includes:

- **Domain and access security setup**: Configure Route53 hosted zones and request ACM certificates to prepare secure application endpoints
- **Network foundation setup**: Create the VPC, target group, Application Load Balancer, and security groups to establish the traffic path between users and backend services
- **Authentication setup**: Configure the Cognito user pool, app client, and hosted UI for user sign-in and identity management
- **Frontend delivery setup**: Prepare the S3 bucket, Origin Access Control, and CloudFront distribution to deploy the static frontend securely
- **Data layer setup**: Provision RDS for application data and Redis for caching to improve performance

By the end of the workshop, the LunchSync environment will have the core AWS services needed for access, authentication, data storage, and performance optimization in the cloud.

---
title: "Introduction"
date: 2026-04-04
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

#### Overview

This section introduces the overall architecture and the AWS services used in the **LunchSync** deployment process.

#### System components

The LunchSync system is deployed on AWS with the following main components:

- **Route53 and ACM** to manage DNS and provision SSL/TLS certificates for the application domain
- **Amazon Cognito** to support user authentication and secure sign-in flows
- **VPC, Security Group, Target Group, and ALB** to build the network layer, control access, and route traffic to backend services
- **Amazon ECR and AWS Fargate** to store container images and run the backend deployment flow
- **Amazon RDS** to provide the relational database layer
- **Redis** to add caching and improve application response performance

#### Workshop Architecture

![Workshop architecture](/images/2-Proposal/lunchsync.png)

#### Workshop overview

In this workshop, you will deploy a multi-stage LunchSync environment on AWS by building each infrastructure layer step by step. It includes:

- **Access and authentication setup**: Configure Route53 hosted zones, request ACM certificates, and set up Cognito for secure access and sign-in
- **Network foundation setup**: Create the VPC, target group, Application Load Balancer, and security groups to establish the traffic path between users and backend services
- **Container pipeline and runtime setup**: Prepare ECR, connect the image build flow to source code, and deploy the backend on ECS/Fargate
- **Data layer setup**: Provision RDS for application data and Redis for caching to improve performance

By the end of the workshop, the LunchSync environment will have the core AWS services needed for access, authentication, backend container deployment, data storage, and performance optimization in the cloud.

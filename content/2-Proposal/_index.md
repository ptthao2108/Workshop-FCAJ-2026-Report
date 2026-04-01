---
title: "Proposal"
date: 2026-03-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Lunch Sync PLATFORM for Lunch Decision  
## Cloud-Native AWS Solution for Meal Decision Support  

### 1. Executive Summary  
- **LunchSync** is a **real-time group decision-support platform**, specifically designed for office workers in high-density urban areas (District 1, Ho Chi Minh City). The project solves the **"what to eat for lunch"** problem by digitizing the group negotiation process through **gamified voting mechanisms** and **context-aware recommendation algorithms**.  

- Unlike food delivery applications that focus on logistics, **LunchSync does not sell food—it sells “decisions”**, addressing both **preference conflicts** and **decision fatigue** during lunch breaks. With a **container-based serverless architecture (Fargate) on AWS**, the system ensures **high concurrency handling during peak hours**, reduces decision time from 20 minutes to **3 minutes**, and **optimizes users’ valuable lunch breaks**.  

---

### 2. Problem Statement  

#### Current Problems  
- **Decision fatigue:** After intense work hours, users **lack the energy to decide what to eat**.  
- **Conflict of preferences:** Larger groups → **diverse tastes → difficult to reach consensus**.  
- **Resource wastage:** Spending 15–20 minutes choosing a place → **reduces break time and productivity**.  
- **Low social friction:** Shared dining decisions **reduce individual responsibility pressure**, as outcomes are collective experiences.  

#### Solution  
- **Core idea:** A mobile application focused on **real-time voting + elimination game**, enabling groups to **finalize a restaurant in ~90 seconds**.  

- **How it works:**  
  - Quickly create a **session**.  
  - Users select preferences via **binary choices (10 food preference dimensions)**—focusing on taste criteria instead of debating dishes.  
  - The system applies:  
    - **Weighted Scoring** → filtering & ranking restaurants.  
    - **"Boom" (random elimination)** → forcing a final decision.  

> Users select preferences across 10 food dimensions: broth level, temperature, heaviness, flavor intensity, spiciness, texture, eating time, novelty, healthiness, and shared vs. individual dining.  

- **MVP Goals:**  
  - Handle **high concurrency** (multiple users voting simultaneously).  
  - Process **accurate geospatial data** (grouping restaurants by area/landmark).  
  - Deliver a **minimal UX**, optimized for decision speed.  

- **Technical Implementation:**  
  - Architecture: **Modular Monolith**  
  - Design: **Clean Architecture** → maintainable & extensible  
  - Backend: **ASP.NET**  
  - Infrastructure: Fully deployed on **AWS**  

> MVP real-time uses **short polling (3s interval)** instead of WebSocket to reduce complexity. WebSocket will be considered in Phase 2.  

---

#### Benefits & Return on Investment (ROI)  

- **Security & Stability:** Private Subnet + Auto Scaling → ensures **high availability**, minimizes downtime.  
- **Performance:** Flexible scaling during peak hours, low latency via CDN and caching.  
- **Development Focus:** Managed services reduce operational burden (Ops), accelerating MVP delivery.  

- **Infrastructure Cost (Baseline):**  
  - ~55.75 USD/month (~1,400,000 VND) for core services (Fargate, WAF, Route 53, Multi-AZ).  

#### ROI Summary (SaaS)  
> The ROI table below assumes **scaling after achieving product-market fit** and transitioning to a SaaS/Freemium model.  

| Metric                    | Value           |
| ------------------------- | --------------- |
| Initial Investment        | $5,000          |
| AWS Cost / Month          | ~$56            |
| Revenue (100 users)       | $2,000 / month  |
| Net Profit                | ~$1,444 / month |
| Payback Period            | ~3.4 months     |
| ROI (1 year)              | ~346%           |
| Revenue (1,000 users)     | $20,000 / month |
| AWS Cost (1,000 users)    | ~$180 / month   |
| Profit Margin             | ~97% → ~99%     |

> With ~55.75–71.75 USD/month, the system achieves **High Availability (Multi-AZ)** and **automatic failover**. If one Availability Zone fails, the system switches to another within seconds, ensuring continuous service.  

---

### 3. Solution Architecture  

The system is built on Amazon Web Services using a cloud-native architecture. Amazon ECS Fargate is used to deploy the backend with a container-based serverless model, eliminating infrastructure management.  

The application follows a Multi-AZ deployment combined with Auto Scaling to ensure High Availability and flexible scalability during peak hours. Traffic is distributed via an Application Load Balancer (ALB).  

The frontend is hosted on Amazon S3 and delivered through CloudFront CDN to optimize latency. Security is enforced using AWS WAF, Amazon Cognito, and Security Groups for access control and protection.  

Data is stored in Amazon RDS (PostgreSQL), while Amazon ElastiCache (Redis) is used for caching and low-latency operations such as session management and voting, reducing backend load. Monitoring is handled by Amazon CloudWatch, and deployment is automated via CI/CD using GitHub Actions, integrated with Amazon ECR and AWS IAM.  

![LunchSync Platform Architecture](/images/2-Proposal/lunchsync.png)

*AWS Services Used*  
- **Amazon ALB (Application Load Balancer):** Distributes HTTP/HTTPS traffic to backend services.  
- **Amazon ECS Fargate:** Runs backend containers in a serverless model.  
- **Amazon S3:** Stores static assets and hosts the frontend.  
- **Amazon CloudFront:** CDN for low-latency content delivery.  
- **Amazon Cognito:** Handles authentication and user authorization.  
- **AWS WAF (with CloudFront):** Protects against common web attacks.  
- **Amazon RDS (PostgreSQL):** Stores relational data (users, restaurants, sessions, etc.).  
- **Amazon ElastiCache (Redis):** Caches hot data, processes voting, reduces system load.  
- **Amazon CloudWatch:** Monitoring (logs, metrics, alarms).  
- **Amazon ECR:** Stores Docker images for deployment.  
- **AWS IAM:** Manages permissions across services and CI/CD pipelines.  

---

### 4. MVP Scope  

| Aspect             | In Scope                                                                | Out of Scope                     |
| ------------------ | ----------------------------------------------------------------------- | -------------------------------- |
| Meal type          | Lunch only                                                              | Dinner, breakfast, cafe          |
| Platform           | React Web App (mobile-first)                                            | Native mobile app                |
| Backend            | ASP.NET Pragmatic Modular Monolith                                      | Microservices                    |
| Database           | PostgreSQL                                                              | NoSQL                            |
| Vector computation | In .NET (basic math)                                                    | Python external service          |
| Group size         | 3–8 users                                                               | >8                               |
| Auth               | Host requires account (simple JWT - Cognito); participants join via nickname | Social login (if time allows) |
| Data               | Manual curation                                                         | Auto-crawling                    |
| Geography          | Single area (3–5 collections)                                           | Multi-city                       |
| Monetization       | Fully free                                                              | Paid plans                       |
| Dietary            | No filtering                                                            | Vegan, allergy, halal            |
| Restaurant action  | Google Maps link                                                        | Reservation, review, ordering    |

---

### 5. Roadmap & Milestones  

- *Internship Phase (Jan–Mar):*  
  - January: Learn AWS, explore infrastructure and technologies.  
  - February: Design and refine system architecture.  
  - March: Implement, test, and deploy.  

- *Post-deployment:*  
  - Continue research and improvement for 1 year.  

---

### 6. Budget Estimation  

- **Amazon Route 53:** $0.50/month (1 Hosted Zone, <10,000 queries)  
- **AWS WAF:** $7.60/month (1 WebACL, 2 basic rules, <1M requests)  
- **Amazon ECS (Fargate):** $18.00/month (2 tasks running 24/7 across 2 AZs)  
- **Amazon S3:** $0.05/month (1 GB storage, <10,000 requests)  
- **Amazon CloudFront:** $0.00/month (<1 TB transfer – Always Free)  
- **Amazon Cognito:** $0.00/month (~100 MAU – Always Free)  
- **Application Load Balancer (ALB):** $0.00/month (Free Tier) (~$16/month after Free Tier)  
- **Amazon RDS (PostgreSQL Multi-AZ):** ~$18.00/month (replica + doubled storage)  
- **Amazon ElastiCache (Redis Multi-AZ):** ~$11.50/month (2 nodes, standby charged)  
- **Amazon ECR:** $0.00/month (<500 MB storage)  
- **Cross-AZ Data Transfer:** ~$0.10/month  

---

#### **Total Estimated Cost**  
**~$55.75 – $71.75/month (~1,400,000 – 1,890,000 VND)**  

---

### 7. Risk Assessment  

| Risk                                    | Level        | Mitigation Strategy               | Contingency Strategy              |
| --------------------------------------- | ------------ | ---------------------------------- | ---------------------------------- |
| **Infrastructure / Uptime (AZ failure)** | Low          | Multi-AZ + ALB failover            | Not required (handled by AWS)      |
| **Security (DDoS, SQLi)**               | Low          | WAF + Cognito + Private Subnet     | Rotate credentials, isolate system |
| **Deployment errors (prod bugs)**       | Medium       | CI/CD + automated tests            | Rollback (ECS task revision)       |
| **Cost overrun (spike)**                | High 🔴       | Auto Scaling limits, WAF rate limit| AWS Budgets + billing alarms       |
| **Performance at scale**                | Medium       | Redis cache, CloudFront CDN        | Scale DB / read replicas           |

---

### 8. Expected Outcomes  

- **Infrastructure:** Cloud-native architecture (Multi-AZ, Auto Scaling) ensures high availability, scalability, and reduced downtime risk.  
- **Operational Efficiency:** Managed services (Fargate, RDS, CloudFront) reduce DevOps overhead, accelerate development, and optimize costs.  
- **Financial Optimization:** Low cost with high profit margins, enabling scalable growth without proportional cost increase.  

> Build a stable, scalable SaaS platform with strong long-term growth potential and high cost efficiency.
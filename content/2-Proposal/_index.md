---
title: "Proposal"
date: 2026-03-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Lunch Sync PLATFORM for Lunch Decision  
## A Cloud-Native AWS Solution for Group Meal Decision-Making  

### 1. Executive Summary  
- **LunchSync** is a **real-time group decision-making platform** designed specifically for office workers in high-density urban areas (e.g., District 1, Ho Chi Minh City). The project addresses the everyday problem of **“what to eat for lunch”** by digitizing the group negotiation process through **gamified voting** and a **context-aware recommendation algorithm**.  

- Unlike food delivery applications that focus on logistics, **LunchSync does not sell food—it sells decisions**, solving both **preference conflicts** and **decision fatigue** during lunch breaks. Built on a **container-based serverless architecture (AWS Fargate)**, the system ensures **high concurrency handling during peak hours**, reducing decision time from 20 minutes to **under 3 minutes**, and optimizing users’ valuable lunch breaks.  

---

### 2. Problem Statement  

#### Current Problems  
- **Decision fatigue:** After stressful work hours, users **lack the mental energy to decide what to eat**.  
- **Conflict of preferences:** Groups with diverse tastes → **difficulty reaching consensus**.  
- **Resource wastage:** Spending 15–20 minutes deciding → **reduced break time and productivity**.  
- **Low social friction:** Group decisions **avoid individual responsibility pressure**, since the outcome is shared.  

#### Solution  
- **Core idea:** A mobile-first application using **real-time voting + elimination game mechanics** to help groups finalize a restaurant in ~90 seconds.  

- **How it works:**  
  - Create a session quickly.  
  - Users select preferences via **binary choices (10 food preference dimensions)** instead of debating dishes.  
  - The system applies:  
    - **Weighted Scoring** → filter and rank restaurants.  
    - **“Boom” (random elimination)** → enforce final decision.  

> Users express preferences across 10 food dimensions: soup level, temperature, heaviness, flavor intensity, spiciness, texture, eating time, novelty, healthiness, and sharing style.  

- **MVP Goals:**  
  - Handle **high concurrency** (many users voting simultaneously).  
  - Process **accurate geospatial data** (group restaurants by area/landmark).  
  - Deliver a **minimal UX focused on speed**.  

- **Technical Implementation:**  
  - Architecture: **Modular Monolith**  
  - Design: **Clean Architecture** for maintainability and scalability  
  - Backend: **ASP.NET**  
  - Infrastructure: Fully deployed on **AWS**  

> MVP uses **short polling (3-second interval)** instead of WebSocket to reduce complexity. WebSocket will be considered in Phase 2.  

#### Benefits & ROI  
- **Security & Reliability:** Private Subnet + Auto Scaling → **high availability**, minimal downtime  
- **Performance:** Flexible scaling during peak hours, low latency via CDN and caching  
- **Development Focus:** Managed services reduce operational overhead and accelerate MVP delivery  

- **Cost:**  
  - Core system: ~$78.42/month (~2,000,000 VND) for Fargate, RDS, ElastiCache (Multi-AZ)  
  - Infrastructure & Security (~$131/month):  
    - VPC Endpoints: $94.90  
    - ALB & Public IPs: ~$24.00  
    - Others (WAF, Logs, Route53): ~$12.00  

#### ROI Summary (SaaS)  

> The following table assumes scaling after achieving product-market fit under a SaaS/Freemium model.

| Metric                    | Value             |
| ------------------------ | ----------------- |
| Initial Investment       | $5,000            |
| AWS Cost / Month         | ~$210             |
| Revenue (100 users)      | $2,000 / month    |
| Net Profit               | ~$1,290 / month   |
| Payback Period           | ~3.9 months       |
| ROI (1 year)             | ~309%             |
| Revenue (1,000 users)    | $20,000 / month   |
| AWS Cost (1,000 users)   | ~$350 / month     |
| Profit Margin            | ~93% – 98%        |

> With ~$210/month, the system achieves **High Availability (Multi-AZ)** and **automatic failover**. If one Availability Zone fails, traffic shifts to another within seconds.  

---

### 3. Solution Architecture  

The system is built on **Amazon Web Services (AWS)** using a cloud-native architecture. Backend services run on **Amazon ECS Fargate** with a container-based serverless model, eliminating infrastructure management.  

The application is deployed across **multiple Availability Zones (Multi-AZ)** with **Auto Scaling** to ensure high availability and scalability during peak hours. Traffic is distributed via an **Application Load Balancer (ALB)**.  

Frontend assets are hosted on **Amazon S3** and delivered through **CloudFront CDN** to reduce latency. Security is enforced using **AWS WAF**, **Amazon Cognito**, and **Security Groups**.  

Data is stored in **Amazon RDS (PostgreSQL)**, while **Amazon ElastiCache (Redis)** handles caching and low-latency operations such as sessions and voting. Monitoring is handled by **Amazon CloudWatch**, and CI/CD is implemented via **GitHub Actions**, integrated with **Amazon ECR** and **AWS IAM**.  

![LunchSync Platform Architecture](/images/2-Proposal/lunchsync.png)

#### AWS Services Used  
- **Amazon ALB (Application Load Balancer):** Distributes HTTP/HTTPS traffic  
- **Amazon ECS Fargate:** Serverless container runtime for backend  
- **Amazon S3:** Static asset storage and frontend hosting  
- **Amazon CloudFront:** CDN for low-latency delivery  
- **Amazon Cognito:** Authentication and authorization  
- **AWS WAF:** Protection against common web attacks  
- **Amazon RDS (PostgreSQL):** Relational database  
- **Amazon ElastiCache (Redis):** Caching and real-time processing  
- **Amazon CloudWatch:** Monitoring (logs, metrics, alarms)  
- **Amazon ECR:** Container image registry  
- **AWS IAM:** Access control management  
- **AWS Secrets Manager:** Secure credential storage  
- **VPC Endpoint:** Secure connectivity between private subnet and AWS services  

---

### 4. MVP Scope  

| Aspect             | In Scope                                                                | Out of Scope                     |
| ------------------ | ----------------------------------------------------------------------- | -------------------------------- |
| Meal type          | Lunch only                                                              | Dinner, breakfast, café          |
| Platform           | React Web App (mobile-first)                                            | Native mobile app                |
| Backend            | ASP.NET Pragmatic Modular Monolith                                      | Microservices                    |
| Database           | PostgreSQL                                                              | NoSQL                            |
| Vector computation | In .NET (basic math)                                                    | Python external service          |
| Group size         | 3–8 users                                                               | >8                               |
| Auth               | Host account (JWT - Cognito), participants via nickname                 | Social login (optional)          |
| Data               | Manual curation                                                         | Auto-crawling                    |
| Geography          | Single area (3–5 collections)                                           | Multi-city                       |
| Monetization       | Free                                                                    | Paid plans                       |
| Dietary            | Not included                                                            | Vegan, allergy, halal            |
| Restaurant action  | Google Maps link                                                        | Booking, reviews, ordering       |

---

### 5. Tech Stack  

| Layer           | Technology                               | Hosting                         |
| --------------- | ---------------------------------------- | ------------------------------- |
| Frontend        | React + TypeScript + Vite + Tailwind CSS | S3 + CloudFront                 |
| Backend         | ASP.NET Web API + EF Core                | ECS Fargate                     |
| Database        | PostgreSQL 15+                           | AWS RDS (db.t3.micro Free Tier) |
| Media Storage   | —                                        | AWS S3                          |
| Auth (Phase 1)  | Simple JWT                               | Amazon Cognito                  |
| CI/CD           | GitHub Actions                           | Build + Deploy                  |
| Version Control | Git — GitHub                             | —                               |

---

### 6. Key Features  

**Group Voting Lunch Restaurant Flow**  

**PHASE 0: SETUP (Host)**  
- Create group session (3–8 users)  
- Set budget cap (e.g., 50,000 VND/person)  
- Select landmark-based collection  

**PHASE 1: INDIVIDUAL BINARY CHOICE**  
- Each user answers 7–12 binary questions  
- Generates a **preference vector** per user  

**PHASE 2: AGGREGATE & SUGGEST**  
- Merge vectors into a session-level vector  
- Score all dishes → select top 10  
- Filter dishes available in the selected collection  
- Suggest top 3 dishes  

**PHASE 3: RESTAURANT MATCHING**  
- Rank restaurants based on collection + budget  
- Suggest top 5 restaurants  

**PHASE 4: GROUP PICK**  
- Group vetoes 2 → finalize 1 restaurant  
- Decision via voting or host  

---

### 7. Roadmap & Milestones  

- **Internship (Months 1–3):**  
  - Month 1: Learn AWS and core services  
  - Month 2: Design and refine architecture  
  - Month 3: Implementation, testing, deployment  

- **Post-deployment:**  
  - Continue research and improvements over 1 year  

---

### 8. Budget Estimation  

### Full System Cost (2 AZ – 30 Days)

| Service Group  | Component                  | Calculation                              | Cost (USD/Month) |
| -------------- | -------------------------- | ---------------------------------------- | ---------------- |
| VPC Endpoint   | 5 Interface Endpoints      | 5 × 2 AZ × 730h × $0.013                 | $94.90           |
| Load Balancer  | ALB + 2 Public IPs         | ($0.0225 + $0.01) × 730h                 | $23.72           |
| Compute        | ECS Fargate                | 2 tasks (0.25 vCPU - 0.5GB)              | $19.00           |
| Database       | RDS PostgreSQL             | t3.micro (Multi-AZ + 20GB)               | $30–34           |
| Cache          | ElastiCache Redis          | t3.micro (Multi-AZ)                      | $24–26           |
| Monitoring     | CloudWatch + WAF + Secrets | Low usage                                | $10.50           |
| Others         | Route53, S3, ECR, Cognito  | Maintenance                              | $2.00            |
| **TOTAL**      |                            |                                          | **~$204–$210**   |

---

### Evaluation  

- **Fault Tolerance:** Extremely high. The system can withstand a full AZ failure without service disruption.  
- **Security:** Highly optimized (private subnet, no public IP, traffic via PrivateLink).  
- **Cost:** ~5.2 million VND/month  

---

### 9. Risk Assessment  

| Risk                                   | Level       | Mitigation Strategy                  | Contingency Plan                     |
| -------------------------------------- | ----------- | ------------------------------------ | ------------------------------------ |
| Infrastructure / Uptime (AZ failure)   | Low         | Multi-AZ + ALB failover              | Not required (handled by AWS)        |
| Security (DDoS, SQLi)                  | Low         | WAF + Cognito + Private Subnet       | Rotate credentials, isolate system   |
| Deployment bugs                        | Medium      | CI/CD + automated testing            | Rollback (ECS task revision)         |
| Cost overrun                           | High 🔴      | Auto Scaling limits, WAF rate limit  | AWS Budgets + billing alerts         |
| Performance at scale                   | Medium      | Redis cache, CDN                     | Scale DB / add read replicas         |

---

### 10. Expected Outcomes  

- **Infrastructure:** Cloud-native architecture (Multi-AZ, Auto Scaling) ensures high availability, scalability, and reduced downtime risk  
- **Operational Efficiency:** Managed services reduce DevOps workload and accelerate development  
- **Financial Efficiency:** Low cost with high profit margins, enabling scalable growth  

> Build a stable, scalable SaaS platform with strong long-term growth potential and cost efficiency.
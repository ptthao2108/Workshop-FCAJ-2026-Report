---
title: "Proposal"
date: 2026-03-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# LunchSync PLATFORM for Lunch Decision  
## A Cloud-Native Solution on AWS for Meal Decision-Making  

### 1. Executive Summary  
- **LunchSync** is a **real-time group decision-making platform**, specifically designed for office workers in high-density urban areas (District 1, Ho Chi Minh City). The project solves the problem of **“what to eat for lunch”** by digitizing group negotiation through a **gamified voting mechanism** and a **context-aware recommendation algorithm**.

- Unlike food delivery applications that focus on logistics, **LunchSync does not sell food — it sells “decisions”**, addressing both **preference conflicts** and **decision fatigue** during lunch breaks. Built on a **container-based serverless architecture (AWS Fargate)**, the system ensures **high concurrency handling during peak hours**, reducing decision time from 20 minutes to **just 3 minutes**, thereby optimizing users’ valuable lunch breaks.  

---

### 2. Problem Statement  

#### Current Problems  
- **Decision fatigue:** After work, users **lack the energy to decide what to eat**.  
- **Conflict of preferences:** Groups → **different tastes → difficult to decide**.  
- **Resource wastage:** 15–20 minutes wasted choosing → **reduced rest time and productivity**.  
- **Low social friction:** Shared decision → **no individual pressure**, as it’s a group experience.  

#### Solution  
- **Core idea:** A mobile-first application using **real-time voting + elimination game** to help groups **decide within ~90 seconds**.

- **How it works:**
  - Create a quick **session**  
  - Users answer **binary choices (10 food preference dimensions)** instead of debating dishes  
  - System processes:
    - **Weighted Scoring** → filter & rank options  
    - **“Boom” (random elimination)** → enforce final decision  

> Users express preferences across 10 dimensions: broth level, temperature, heaviness, flavor intensity, spiciness, texture, eating time, novelty, healthiness, and sharing style.

#### MVP Goals  
- Handle **high concurrency** (multiple users voting simultaneously)  
- Process **accurate geospatial data** (restaurant grouping by area/landmark)  
- Deliver **minimal UX**, optimized for speed  

#### Technical Implementation  
- Architecture: **Modular Monolith**  
- Design: **Clean Architecture**  
- Backend: **ASP.NET**  
- Infrastructure: fully deployed on **AWS**  

> MVP uses **short polling (3s interval)** instead of WebSocket to reduce complexity. WebSocket will be considered in Phase 2.

---

### Benefits & Return on Investment (ROI)  

- **Security & Stability:** Private Subnet + Auto Scaling → **high availability**, minimal downtime  
- **Performance:** Scales with peak hours, low latency via CDN & caching  
- **Development Focus:** Managed services reduce Ops overhead → faster MVP delivery  

- **Infrastructure Cost (Baseline):**  
  - ~55.75 USD/month (~1,400,000 VND)  

#### ROI Summary (SaaS)  

> The table below assumes scaling after achieving product-market fit.

| Metric                    | Value           |
|--------------------------|-----------------|
| Initial Investment       | $5,000          |
| AWS Cost / Month         | ~$56            |
| Revenue (100 users)      | $2,000 / month  |
| Net Profit               | ~$1,444 / month |
| Payback Period           | ~3.4 months     |
| ROI (1 year)             | ~346%           |
| Revenue (1,000 users)    | $20,000 / month |
| AWS Cost (1,000 users)   | ~$180 / month   |
| Profit Margin            | ~97% → ~99%     |

> With ~$55.75–$71.75/month, the system achieves **High Availability (Multi-AZ)** and **automatic failover**.

---

### 3. Solution Architecture  

The system is built on **Amazon Web Services (AWS)** using a cloud-native architecture. Backend services run on **Amazon ECS Fargate**, following a container-based serverless model.

- **Multi-AZ + Auto Scaling** ensures high availability  
- **Application Load Balancer (ALB)** distributes traffic  
- **Frontend** hosted on **S3 + CloudFront CDN**  
- **Security:** AWS WAF, Cognito, Security Groups  
- **Database:** Amazon RDS (PostgreSQL)  
- **Caching:** Amazon ElastiCache (Redis)  
- **Monitoring:** CloudWatch  
- **CI/CD:** GitHub Actions + ECR + IAM  

![LunchSync Platform Architecture](/images/2-Proposal/lunchsync.png)

#### AWS Services Used  
- **ALB:** Traffic distribution  
- **ECS Fargate:** Backend compute  
- **S3:** Static storage  
- **CloudFront:** CDN  
- **Cognito:** Authentication  
- **WAF:** Web protection  
- **RDS (PostgreSQL):** Relational database  
- **ElastiCache (Redis):** Caching & voting  
- **CloudWatch:** Monitoring  
- **ECR:** Container registry  
- **IAM:** Access control  
- **Secrets Manager:** Credential management  

---

### 4. MVP Scope  

| Aspect             | In Scope                                              | Out of Scope                     |
|------------------|------------------------------------------------------|--------------------------------|
| Meal type        | Lunch only                                           | Dinner, breakfast, cafe        |
| Platform         | React Web App (mobile-first)                         | Native mobile app              |
| Backend          | ASP.NET Modular Monolith                             | Microservices                  |
| Database         | PostgreSQL                                           | NoSQL                          |
| Vector compute   | .NET (basic math)                                    | Python external service        |
| Group size       | 3–8 users                                            | >8                             |
| Auth             | Host account (JWT + Cognito), guest nickname         | Social login                   |
| Data             | Manual curation                                      | Auto crawling                  |
| Geography        | Single area                                          | Multi-city                     |
| Monetization     | Free                                                 | Paid plans                     |
| Dietary          | None                                                 | Vegan, allergy, halal          |
| Restaurant action| Google Maps link                                     | Booking, reviews, ordering     |

---

### 5. Tech Stack  

| Layer           | Technology                               | Hosting                         |
|----------------|------------------------------------------|----------------------------------|
| Frontend       | React + TypeScript + Vite + Tailwind CSS | S3 + CloudFront                 |
| Backend        | ASP.NET Web API + EF Core                | ECS Fargate                     |
| Database       | PostgreSQL 15+                           | AWS RDS (db.t3.micro Free Tier) |
| Media Storage  | —                                        | AWS S3                          |
| Auth (Phase 1) | Simple JWT                               | Amazon Cognito                  |
| CI/CD          | GitHub Actions                           | Build + Deploy                  |
| Version Control| Git — GitHub                             | —                               |

---

### 6. Key Features  

#### Group Voting Flow  

**PHASE 0: Setup (Host)**  
- Create session (3–8 users)  
- Set budget  
- Select lunch collection  

**PHASE 1: Individual Binary Choice**  
- 7–12 fixed questions  
- Each user generates a **preference vector**  

**PHASE 2: Aggregate & Suggest**  
- Merge vectors → final vector  
- Score dishes → top 10  
- Filter → top 3 dishes  

**PHASE 3: Restaurant Matching**  
- Match & rank restaurants  
- Suggest top 5  

**PHASE 4: Group Pick**  
- Eliminate 2 → choose final restaurant  

---

### 7. Roadmap  

- **Internship (3 months):**  
  - Month 1: AWS learning  
  - Month 2: Architecture design  
  - Month 3: Deployment & testing  

- **Post-launch:**  
  - 1-year research & improvement  

---

### 8. Budget Estimate  

- Route 53: $0.50  
- WAF: $7.60  
- ECS Fargate: $18.00  
- S3: $0.05  
- CloudFront: Free  
- Cognito: Free  
- ALB: Free Tier (~$16 after)  
- RDS Multi-AZ: ~$18.00  
- ElastiCache: ~$11.50  
- ECR: Free  
- Data Transfer: ~$0.10  

---

### **Total Estimated Cost**  
**~55.75 – 71.75 USD/month**

---

### 9. Risk Assessment  

| Risk                | Level        | Mitigation                    | Contingency              |
|---------------------|-------------|------------------------------|--------------------------|
| Infrastructure      | Low         | Multi-AZ                     | AWS handles              |
| Security            | Low         | WAF + Cognito                | Rotate credentials       |
| Deployment bugs     | Medium      | CI/CD + testing              | Rollback                 |
| Cost spike          | High 🔴      | Scaling limits               | Budget alerts            |
| Performance         | Medium      | Cache + CDN                  | DB scaling               |

---

### 10. Expected Outcomes  

- **Infrastructure:** Highly available, scalable cloud-native system  
- **Operations:** Reduced DevOps overhead via managed services  
- **Financial:** Low cost, high scalability potential  

> Build a scalable, cost-efficient SaaS platform with strong long-term growth potential.
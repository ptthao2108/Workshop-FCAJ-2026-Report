---
title: "Event 4"
date: 2026-04-04
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Summary Report: "Cloud Native & Infrastructure - Kubernetes, Elixir & IaC"

### Event Objectives

- Introduce core ideas behind cloud-native platforms and modern infrastructure
- Highlight practical concepts related to Kubernetes, DevOps, and Infrastructure as Code
- Present architectural thinking for scalable and resilient systems
- Give attendees a clearer picture of how production environments are built and maintained

### Speakers

- **Bao Huynh** - Junior Cloud Native Developer, Endava Vietnam; Founder / Head Lab, ITea Lab
- **Nguyen Ta Minh Triet** - R&D Member at ITea Lab, Swinburne Vietnam; SAP Developer Intern, Bosch GSV
- **Thinh Nguyen** - FCAJ Cloud Engineer Trainee

### Key Highlights

#### 1. Kubernetes and Amazon EKS

**What is Kubernetes**

- Kubernetes is a platform for orchestrating containers
- Helps automate application deployment
- Supports horizontal scaling
- Improves self-healing capability
- Handles service exposure and load balancing

Kubernetes can be understood as a control layer for distributed applications.

**Amazon EKS**

- EKS is AWS's managed Kubernetes offering
- AWS is responsible for the control plane, including the API server, etcd, and availability setup
- Users are responsible for worker nodes, deployed services, and application workloads

**Why use EKS**

- Saves time compared with setting up Kubernetes from scratch
- Works naturally with AWS services and networking
- Makes it easier to reach a production-ready environment

**Why still use vanilla Kubernetes**

- Reduce dependence on a single cloud vendor
- Fit multi-cloud or hybrid deployment models
- Provide more room for low-level customization

**High-level EKS architecture**

- Control plane is operated by AWS
- Worker nodes can run on EC2 or Fargate
- Network components are organized inside a VPC
- Traffic can be exposed through ALB or NLB

**Main insight**

- EKS is suitable when teams want faster setup and lower operational overhead
- Vanilla Kubernetes is stronger when flexibility and fine-grained control matter more
- The practical lesson is to select the trade-off that matches the system context

#### 2. Elixir, BEAM, and Fault Tolerance

**Elixir and BEAM**

- Elixir is a functional programming language
- It runs on BEAM, the runtime originally built for Erlang systems

**Process model**

- Uses lightweight processes instead of traditional threads
- Avoids shared memory between processes
- Relies on message passing for communication

Benefits:

- Less need for locking
- Lower risk of race conditions
- Strong support for highly concurrent workloads

**Fault tolerance**

- A central idea is "Let it crash"
- Rather than over-handling every failure, the system is designed to restart failed parts safely

**OTP**

- Provides supervisor trees
- Provides GenServer
- Provides restart strategies

These features make recovery behavior part of the system design.

**Process supervision**

- Workers handle specific tasks
- Supervisors watch over workers
- When a worker stops unexpectedly, the supervisor can restart it

**Hot code upgrade**

- Elixir supports replacing code with minimal or no downtime
- In current production practice, this idea is often paired with rolling or blue-green deployment strategies on Kubernetes

**DevOps and Elixir**

Tools mentioned in the event:

- Mix for building projects
- Hex for dependency management
- ExUnit for testing
- Observer for runtime monitoring

**Main insight**

- Reliable systems are not built only by trying to avoid failure
- Elixir promotes a resilience-first approach in which recovery is a built-in expectation

#### 3. Infrastructure as Code

**What is IaC**

- Infrastructure as Code is the practice of defining and managing infrastructure through source code instead of manual configuration

Benefits:

- Better automation
- Easier reproducibility
- Clearer version tracking

**Terraform**

- Works across multiple cloud providers
- Uses a declarative style to describe the target infrastructure state

Typical basic structure:

- `main.tf`
- `variables.tf`
- `outputs.tf`
- `providers.tf`

Advanced structure may include:

- `modules` for reusable infrastructure components
- `environment` folders such as dev, staging, and prod
- `bootstrap` for initial backend or state setup

**AWS CloudFormation**

- Native IaC service from AWS
- Uses YAML or JSON templates
- Also follows a declarative approach

**AWS CDK**

- Allows infrastructure definition in languages such as TypeScript or Python
- Organizes code with the App -> Stack -> Construct model

**CDK vs Terraform**

- CDK is convenient for developers working deeply in the AWS ecosystem
- Terraform is attractive for multi-cloud use cases and reusable ecosystem support

**LocalStack**

- Emulates AWS services in a local environment
- Makes testing cheaper and more convenient before using real cloud resources

**Main insight**

- Choosing a tool is only one part of the problem
- Long-term effectiveness depends more on project structure, consistency, and team discipline

### What I Learned

#### Technology Mindset

- Understood cloud-native architecture from a more system-level perspective
- Saw more clearly that engineering decisions are based on trade-offs, not absolute answers
- Learned to view resilience as a design principle instead of an afterthought

#### Technical Knowledge

Became familiar with:

- Kubernetes and Amazon EKS
- Fault-tolerant systems with Elixir and BEAM
- Infrastructure as Code

Gained a basic understanding of how to:

- Set up and operate services in production-like environments
- Organize infrastructure management through code

#### Applying to Work

Can apply:

- IaC thinking to infrastructure management tasks
- Container orchestration concepts when working with cloud deployments

In the future:

- Build systems with better scalability and maintainability
- Connect Kubernetes and DevOps practices more directly to project implementation

### Event Experience

This event gave me a broader view of how cloud-native systems are assembled from both the application side and the infrastructure side. Instead of focusing only on individual tools, the sessions showed how Kubernetes, Elixir, and IaC contribute to building stable and production-ready platforms.

I found the discussion around EKS and infrastructure automation especially practical because it connected architecture choices with deployment and operational concerns. The Elixir and BEAM talk was also valuable because it introduced a different way of thinking about failure, where recovery is designed into the system from the beginning.

Some topics were still quite advanced for a first exposure, so I currently understand them more at the conceptual level than at the implementation level. Even so, the event provided a useful foundation for continued learning.

### Lessons Learned

- Technology choices should be evaluated through trade-offs rather than trends
- Production systems need recovery strategies, not only prevention strategies
- Managing infrastructure as code improves consistency and repeatability
- Cloud-native engineering requires both development knowledge and operational thinking

### Some Event Photos

![Event photo 1](/images/events/event4_1.jpg)

![Event photo 2](/images/events/event4_2.jpg)

![Event photo 3](/images/events/event4_3.jpg)

---
title: "Configure security groups"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4.3.4. </b> "
---

1. Open the **Security Groups** section and review the groups used by the application resources.

![SG step 1](sg.1.jpg)

2. Configure inbound rules for the ALB-facing security group.

![SG step 2](sg.2.jpg)

3. Configure the backend security group so it accepts traffic from the ALB or trusted sources only.

![SG step 3](sg.3.jpg)

4. Review the final rule set and save the security group configuration.

![SG step 4](sg.4.jpg)

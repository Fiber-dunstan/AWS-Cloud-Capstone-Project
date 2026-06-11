# AWS-Cloud-Capstone-Project

AWS Cloud Capstone Project

### Architecture
Browser → CloudFront → ALB → EC2 → S3

### AWS Services Used
- EC2 (t2.micro) - Web server
- S3 - Static assets
- ALB - Load balancing
- CloudFront - CDN and HTTPS
- ACM - SSL certificate
- IAM - Access management
- CloudWatch - Monitoring

### Setup Guide
1. Clone this repository
2. Configure AWS CLI with your credentials
3. Follow deployment steps in /docs folder

### Team
- Hillary
- Audrey
- Millicent
- Sandra
- Dunstan

### Phase 1 Progress
- [x] AWS Account Created
- [x] IAM User Created
- [x] AWS CLI Installed
- [x] GitHub Repository Configured
- [x] Trello Board Created
- [x] EC2 Instance Launched
- [x] Apache Installed
- [x] S3 Bucket Created
- [ ] ACM Certificate Requested

### Project Status
Phase 1 Environment Setup In Progressn


## Phase 3 — Security Configuration

In Phase 3, we focused on securing the deployed web application by controlling traffic flow, limiting access to AWS resources, and applying least-privilege permissions across the environment.

### Traffic Flow

The secured traffic flow for the application is:

```text
Browser → CloudFront (HTTPS) → ALB → EC2 (Apache)
                             ↘ S3 (via OAC only)
```

This means users access the application through CloudFront using HTTPS. CloudFront then routes traffic to the Application Load Balancer, which forwards requests to the EC2 instance running Apache. Static content stored in S3 is accessed only through CloudFront using Origin Access Control.

---

### Security Group Rules

#### EC2 Security Group: `webapp-sg`

| Port | Protocol | Source             | Purpose                                     |
| ---- | -------- | ------------------ | ------------------------------------------- |
| 22   | TCP      | My IP only         | Allows secure SSH access for administration |
| 80   | TCP      | ALB Security Group | Allows web traffic from the ALB only        |
| 443  | TCP      | ALB Security Group | Allows HTTPS traffic from the ALB only      |

The EC2 instance does not accept public web traffic directly. Only the Application Load Balancer is allowed to forward traffic to the instance.

#### ALB Security Group

| Port | Protocol | Source                 | Purpose                                   |
| ---- | -------- | ---------------------- | ----------------------------------------- |
| 80   | TCP      | CloudFront Prefix List | Allows HTTP traffic from CloudFront only  |
| 443  | TCP      | CloudFront Prefix List | Allows HTTPS traffic from CloudFront only |

The ALB is restricted to receive traffic only from CloudFront. This prevents users from bypassing CloudFront and accessing the load balancer directly.

---

### IAM Configuration

| Resource           | Policy / Role                     | Reason                                                          |
| ------------------ | --------------------------------- | --------------------------------------------------------------- |
| `cloud-admin` user | `capstone-least-privilege-policy` | Grants only the minimum permissions required for the project    |
| EC2 instance       | `capstone-ec2-s3-role`            | Allows the EC2 instance to access S3 securely using an IAM role |

IAM permissions were configured using the principle of least privilege. This ensures that users and services only have the permissions they need to perform their tasks.

---

### S3 Security

The S3 bucket was secured to prevent direct public access.

* Public access is completely blocked.
* Access is allowed only through CloudFront Origin Access Control.
* The bucket policy allows access only to the CloudFront service principal.
* Users cannot access the S3 bucket directly from the internet.

This helps protect static files and ensures that all public access goes through CloudFront.

---

### CloudFront Security

CloudFront was configured to improve security and control how users access the application.

* HTTPS is enforced.
* HTTP requests are redirected to HTTPS.
* The SSL certificate was created using AWS Certificate Manager in the `us-east-1` region.
* A custom header, `X-CloudFront-Secret`, was added for origin verification.
* The price class was limited to North America and Europe.

This setup ensures secure communication between users and the application while reducing unnecessary exposure of backend resources.

---

### Summary

In this phase, we improved the security of the application by placing CloudFront in front of the ALB, restricting traffic using security groups, blocking public access to S3, and using IAM roles and least-privilege policies. This helped create a more secure and controlled architecture for the deployed web application.

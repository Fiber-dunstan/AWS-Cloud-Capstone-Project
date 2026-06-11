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
## Phase 3 — Security Configuration

### Traffic Flow
Browser → CloudFront (HTTPS) → ALB → EC2 (Apache)
                             ↘ S3 (via OAC only)

### Security Group Rules

#### EC2 (webapp-sg)
| Port | Protocol | Source | Purpose |
|------|----------|--------|---------|
| 22 | TCP | My IP only | SSH admin access |
| 80 | TCP | ALB Security Group | Web traffic from ALB only |
| 443 | TCP | ALB Security Group | HTTPS from ALB only |

#### ALB Security Group
| Port | Protocol | Source | Purpose |
|------|----------|--------|---------|
| 80 | TCP | CloudFront Prefix List | HTTP from CloudFront only |
| 443 | TCP | CloudFront Prefix List | HTTPS from CloudFront only |

### IAM Configuration
| Resource | Policy | Reason |
|----------|--------|--------|
| cloud-admin user | capstone-least-privilege-policy | Minimum required permissions |
| EC2 instance | capstone-ec2-s3-role | Read-only S3 access via role |

### S3 Security
- Public access: Blocked completely
- Access method: CloudFront OAC only
- Bucket policy: Only allows CloudFront service principal

### CloudFront Security
- Protocol: HTTPS only (HTTP redirects to HTTPS)
- SSL Certificate: ACM certificate (us-east-1)
- Custom header: X-CloudFront-Secret for origin verification
- Price class: North America and Europe only
### Team
Hillary
Audrey
Millicent
Sandra
Block All Public Access to S3Dunstan

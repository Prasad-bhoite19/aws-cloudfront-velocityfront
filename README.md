# 🚀 VelocityFront — Next-Gen Global CDN Accelerator (CloudFront Pro Guide)

🌐⚡ AWS CloudFront — Modern, Visual, & Production-Ready Deep-Dive

🎯 A professional, fully-visual, beautifully structured CloudFront guide designed for GitHub, recruiters, and real DevOps portfolios.

**This repo includes diagrams, workflows, CLI, JSON, YAML, architecture, optimization techniques, troubleshooting, and real-world CloudFront distribution examples.**


## ⚙️ This guide is built with premium DevOps documentation standards:-

- ✨ Fully visual documentation
- ✨ Cloud architecture diagrams (ASCII + flowcharts)
- ✨ Multi-origin CloudFront distribution examples
- ✨ Console + CLI + CloudFormation setup
- ✨ Security best practices
- ✨ Performance tuning

## 📚 Table of Contents:-

- 🔥 Overview
- 🌐 How CloudFront Works
- 🧬 High-Level Flow (Explained Simply)
- 🧬 Architecture (ASCII)
- 🏗️ Multi-Origin Architecture (NEW)
- 🔧 Components Explained
- 🧭 Create Distribution (Console)
- 🖥️ Create via AWS CLI
- 📂 Create via CloudFormation
- 🔒 Security Best Practices
- ⚙️ Optimization Techniques
- 🔁 Post-Setup Tasks
- 🛠️ Troubleshooting
- 📁 Useful Commands
- 🚀 Future Enhancements
- ✍️ Author (Prasad)

## 🔥 1. Overview:-

**AWS CloudFront is a global CDN delivering your static/dynamic content with low latency using AWS Edge Locations.**

***This repo explains how to build a production-grade CloudFront setup for:-***

- Static sites (S3)
- ALB/EC2 backends
- API Gateway
- Multi-origin architectures
- Private content (Signed URLs)
- High-security environments

## 🌐 2. How CloudFront Works:-

**CloudFront speeds up content delivery using:-**

- Edge locations
- Global caching
- Intelligent routing
- SSL termination at edge
- Security filtering (WAF, geo-blocking)
- Requests → Closest edge → Cached/Origin fetch → Response

## 🧬 3. High-Level Flow (Explained Simply):-

**✨ This flow shows how CloudFront delivers your content fast:-**

- 🌍 User sends a request
- ⚡ Nearest Edge Location checks cache
- 🔁 If cached → instant response
- 🚫 If not cached → request goes to CloudFront Distribution

**🎯 Distribution forwards to the correct origin:-**

- 🗄️ S3 Bucket
- 🖥️ ALB / EC2
- 🚪 API Gateway
- 📦 Response is cached at Edge Location
- 🚀 Returned to the user with high speed

## 🧱 4. ASCII Architecture (Simple & Clear):-
```
               🌍 GLOBAL USERS
         /        |         \
       UserA    UserB      UserC
            \     |       /
            [ Nearest Edge Location ]
                     |
              CloudFront CDN
                     |
       --------------------------------
       |               |              |
     S3 Origin     ALB / EC2       API Gateway
       |               |              |
  Static Website   Dynamic Apps     REST APIs

```
## 🏗️ 5. Multi-Origin Architecture :-
```
+--------------------------------------------------------+
|                    CloudFront CDN                      |
|                                                        |
|  +-----------------+   +------------------+             |
|  |  Cache Behavior |   |  Cache Behavior  |             |
|  |   /images/*     |   |   /api/*         |             |
|  +-----------------+   +------------------+             |
|         |                       |                      |
|      S3 Bucket              API Gateway                |
|  (Static Images)            (Dynamic API)              |
+--------------------------------------------------------+
```
## 🔧 6. CloudFront Components Explained:-

- Origins: S3, ALB, EC2, API Gateway, on-prem servers
- OAC: Secure S3 access
- Behaviors: Path-based routing
- TLS/SSL: ACM Certificate (us-east-1)
- WAF: Attach security rules
- Logging: Standard logs + real-time logs
- Invalidations: Purge cache instantly

## 🧭 7. Create Distribution (Console Guide):-

### 1️⃣ Go to CloudFront → Create Distribution:-

### 2️⃣ Add Origin Domain:-

- Select S3 / ALB / EC2 / API endpoint

### 3️⃣ Set Origin Access Control (OAC):-

- Recommended for S3
- Auto-updates S3 bucket policy

### 4️⃣ Configure Cache Behavior:-

- HTTP → HTTPS redirect
- GET/HEAD for static
- All methods for APIs
- Choose cache policy:
- CachingOptimized
- CORS-S3Origin

### 5️⃣ Distribution Settings:-

- Alternate domain names (CNAMEs)
- Custom SSL cert (ACM us-east-1)
- Default root object: index.html

### 6️⃣ Optional:-

- Enable logging
- Attach WAF

## 🖥️ 8. AWS CLI Example:-
```
distribution-config.json
{
  "CallerReference": "prasad-cloudfront-2025",
  "Comment": "VelocityFront Distribution",
  "Origins": {
    "Quantity": 1,
    "Items": [
      {
        "Id": "S3Origin",
        "DomainName": "my-bucket.s3.amazonaws.com",
        "S3OriginConfig": { "OriginAccessIdentity": "" }
      }
    ]
  },
  "DefaultCacheBehavior": {
    "TargetOriginId": "S3Origin",
    "ViewerProtocolPolicy": "redirect-to-https",
    "AllowedMethods": { 
      "Quantity": 2, 
      "Items": ["GET", "HEAD"],
      "CachedMethods": { "Quantity": 2, "Items": ["GET", "HEAD"] }
    },
    "Compress": true,
    "ForwardedValues": { "QueryString": false }
  },
  "Enabled": true
}

CLI Command
aws cloudfront create-distribution --distribution-config file://distribution-config.json
```
## 📂 9. CloudFormation Snippet:-
```
Resources:
  PrasadCFN:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Enabled: true
        Comment: "VelocityFront CloudFront"
        Origins:
          - Id: S3Origin
            DomainName: my-bucket.s3.amazonaws.com
            S3OriginConfig: {}
        DefaultCacheBehavior:
          TargetOriginId: S3Origin
          ViewerProtocolPolicy: redirect-to-https
          AllowedMethods:
            - GET
            - HEAD
        PriceClass: PriceClass_100
```
## 🔒 10. Security Best Practices:-

- Use OAC, not public S3
- Enable WAF rules
- Use TLS 1.2+
- Geo-block unused regions
- Enable logging
- Use signed URLs for premium content

## ⚙️ 11. Optimization Techniques:-

- Minimize forwarded headers
- Compress text assets
- Enable HTTP/3
- Cache static aggressively
- Use multiple behaviors (path-based)
- Prefer S3 → CloudFront over EC2 → CloudFront for static content

## 🔁 12. Post-Setup Tasks:-

- Update DNS (Route53 ALIAS → CloudFront)
- Create invalidation rules
- Monitor cache hit ratio
- Check TLS expiration (ACM)

## 🛠️ 13. Troubleshooting:-

**Issue	Fix:-**

- 403 Access Denied	Fix OAC/S3 bucket policy
- Wrong certificate	Use ACM in us-east-1
- Slow load time	Enable compression + caching
- High origin cost	Improve cache hit ratio

## 📁 14. Useful Commands:-
```
- aws cloudfront create-distribution ...
- aws cloudfront list-distributions
- aws cloudfront create-invalidation --distribution-id X --paths "/*"
- aws cloudfront get-distribution --id X
```
## 🚀 15. Future Enhancements:-

- Add CloudFront Functions examples
- Add Lambda@Edge use cases
- Add Terraform version
- Add full CI/CD pipeline for CloudFront deployments
- Convert to a downloadable PDF

## ✍️ Author:-

- 👤 Prasad
- 📌 Cloud & DevOps Engineer
- ⭐ If this repo helped you, please star the repository!

## 🌐 Connect with Me :-

- 🔗 [LinkedIn](http://linkedin.com/in/prasad-bhoite-a38a64223)  
- 🔗 [GitHub](https://github.com/Prasad-bhoite19)  
- 🔗 [Portfolio](https://prasad-bhoite19.github.io/prasad-portfolio/)

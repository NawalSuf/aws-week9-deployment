# AWS Week-9 Assignment: Web Application Deployment

This project showcases the deployment of a static web application using AWS services:
- **Amazon S3** for hosting static assets (HTML + images)
- **Auto Scaling Group (ASG)** to manage EC2 instances
- **Application Load Balancer (ALB)** for distributing incoming traffic
- **Launch Template** with User Data script for EC2 setup

---

## 🔧 Deployment Overview

- Created an S3 bucket: `nawal-clarusway-assets`
- Uploaded:
  - `index.html`
  - `logo.png`
  - `sda.png`
- Enabled Static Website Hosting and configured a public read Bucket Policy.
- Built a Launch Template installing NGINX and pulling assets from S3.
- Configured an Auto Scaling Group:
  - Desired: 2
  - Min: 1
  - Max: 3
- Set up an internet-facing ALB listening on port 80 and attached the Target Group: `clarusway-tg`.

---

## 🐞 Issue Encountered: Missing Images (logo.png and sda.png)

After initial deployment, the main HTML page loaded successfully, but the logo images did not appear.

### 🧪 Root Cause:
- EC2 instances **lacked permissions** to access the S3 bucket.
- User Data tried to copy assets from S3 but failed silently.

---

## ✅ Solution:

1. Created an **IAM Role** with `AmazonS3ReadOnlyAccess`.
2. Attached the role to EC2 instances via **Modify IAM Role** option.
3. Manually copied the assets:

   ```bash
   sudo aws s3 cp s3://nawal-clarusway-assets/index.html /usr/share/nginx/html/
   sudo aws s3 cp s3://nawal-clarusway-assets/logo.png /usr/share/nginx/html/
   sudo aws s3 cp s3://nawal-clarusway-assets/sda.png /usr/share/nginx/html/
Ensured correct file names in index.html (e.g., logo.png, sda.png).

Verified via ALB endpoint — All logos displayed successfully.

🖼️ Final Results
Website accessible via:

S3 Static Website Endpoint (for static assets)

ALB DNS Name (dynamic serving via ASG)

All assets (HTML + logos) are loading properly on both paths.

📂 Files Included
index.html

logo.png

sda.png

Screenshots:

S3 Static Website Hosting

curl -I on S3

ASG Configuration and Instances

ALB access via Browser

Round-Robin curl test on ALB

🖊️ Author
Nawal — SDA Infrastructure Trainee 🚀

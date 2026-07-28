# Space9 Frontend

This repository contains the frontend application for the **Space9 DevOps Platform**.

The application is deployed as a **static website** using the following AWS services:

* Amazon S3
* Amazon CloudFront
* Amazon Route53
* AWS Certificate Manager (ACM)

The deployment is automated using **GitHub Actions** and **GitHub OIDC**.

---

# Architecture

```text
Developer
     │
     ▼
GitHub Repository
     │
     ▼
GitHub Actions
     │
     ▼
Assume IAM Role (OIDC)
     │
     ▼
Upload Static Files to Amazon S3
     │
     ▼
Create CloudFront Invalidation
     │
     ▼
https://dev.space9.in
```

---

# Prerequisites

Before deploying this application, the infrastructure must already exist.

Deploy the following repository first:

```
production-eks-devops-platform
```

This Terraform repository provisions:

* Amazon S3 Bucket
* Amazon CloudFront Distribution
* Amazon Route53 Record
* ACM Certificate
* GitHub OIDC IAM Role

---

# GitHub Repository Variables

Navigate to:

```
Repository
→ Settings
→ Secrets and variables
→ Actions
→ Variables
```

Create the following repository variables.

| Variable                   | Description                | Example                   |
| -------------------------- | -------------------------- | ------------------------- |
| AWS_REGION                 | AWS Region                 | ap-south-1                |
| S3_BUCKET_NAME             | Frontend S3 Bucket         | space9-dev-frontend       |
| CLOUDFRONT_DISTRIBUTION_ID | CloudFront Distribution ID | E2ABCDEF12345             |
| API_URL                    | Backend API URL            | https://dev-api.space9.in |

---

# GitHub Repository Secret

Navigate to:

```
Repository
→ Settings
→ Secrets and variables
→ Actions
→ Secrets
```

Create the following secret.

| Secret       | Description              |
| ------------ | ------------------------ |
| AWS_ROLE_ARN | GitHub OIDC IAM Role ARN |

Example:

```
arn:aws:iam::<ACCOUNT_ID>:role/production-eks-devops-platform-frontend-role
```

---

# Where do these values come from?

After deploying the Terraform infrastructure, execute:

```bash
terraform output
```

Copy the outputs into the GitHub Repository Variables.

Example:

```
s3_bucket_name

cloudfront_distribution_id

api_url
```

The IAM Role ARN can be obtained from:

```bash
terraform output frontend_role_arn
```

---

# Deployment Steps

## Step 1

Clone the repository.

```bash
git clone https://github.com/<github-user>/space9-frontend.git
```

---

## Step 2

Configure GitHub Variables and Secrets.

---

## Step 3

Push your changes to the **dev** branch.

```bash
git add .

git commit -m "Frontend update"

git push origin dev
```

---

## Step 4

GitHub Actions automatically:

* Checks out the repository
* Authenticates to AWS using GitHub OIDC
* Uploads static files to Amazon S3
* Invalidates the CloudFront cache

---

## Step 5

Open the application.

```
https://dev.space9.in
```

---

# Technology Stack

* HTML
* CSS
* JavaScript
* Amazon S3
* Amazon CloudFront
* Amazon Route53
* AWS Certificate Manager (ACM)
* GitHub Actions
* GitHub OIDC

---

# Repository Structure

```
space9-frontend
│
├── .github/
│   └── workflows/
│       └── ci-dev.yaml
│
├── frontend/
│   ├── index.html
│   ├── app.js
│   ├── style.css
│   └── config.js
│
├── .gitignore
├── .dockerignore
└── README.md
```

---

# Notes

* The frontend is deployed as a static website.
* No Docker container is used in production.
* CloudFront is responsible for caching and global content delivery.
* The backend API endpoint is configured using the `API_URL` repository variable.
* Infrastructure provisioning is managed separately in the `production-eks-devops-platform` repository.

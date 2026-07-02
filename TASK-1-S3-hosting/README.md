# TASK-1: S3 Static Website Hosting

## 📌 Objective
Host a static HTML website using **AWS S3**, understand bucket policies,
public access controls, and static website hosting configuration —
from an SRE (Site Reliability Engineering) perspective.

---

## 🗂️ Project Structure

TASK-1-S3-hosting/
├── index.html      # Static webpage served from S3
├── deploy.sh        # (Optional) CLI automation script for deployment
└── README.md         # Documentation for this task

---

## 🛠️ Tools & Services Used
- **AWS S3** (Simple Storage Service)
- **AWS CLI** (optional automation)
- **Git & GitHub** for version control

---

## 🚀 Steps Performed

### 1. Created the project folder
Organized this task in its own directory to keep it isolated from other tasks in the repo.

### 2. Created `index.html`
A simple static webpage designed to confirm successful S3 hosting.

### 3. Created an S3 bucket
- Bucket name: `<your-bucket-name>`
- Region: `ap-south-1` (Mumbai)
- Object Ownership: ACLs disabled (recommended)
- Block Public Access: **disabled** (required for public static hosting)
- Versioning: Enabled (allows rollback of previous file versions)
- Default encryption: SSE-S3 (enabled by default)

### 4. Uploaded `index.html` to the bucket

### 5. Enabled Static Website Hosting
- Index document: `index.html`
- Error document: `index.html`
- Noted the **Bucket website endpoint** URL provided by AWS

### 6. Applied a Bucket Policy for public read access

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

This policy allows **read-only (GetObject)** access to everyone, while
keeping write/delete permissions restricted to the account owner.

### 7. Verified the deployment
Opened the S3 static website endpoint in a browser and confirmed the
page loads successfully.

---

## 💻 AWS CLI Commands (Alternative to Console)

```bash
# Create bucket
aws s3 mb s3://YOUR-BUCKET-NAME --region ap-south-1

# Upload file
aws s3 cp index.html s3://YOUR-BUCKET-NAME/index.html

# Enable static website hosting
aws s3 website s3://YOUR-BUCKET-NAME/ --index-document index.html --error-document index.html

# Apply bucket policy
aws s3api put-bucket-policy --bucket YOUR-BUCKET-NAME --policy file://policy.json
```

---

## 🌐 Live URL
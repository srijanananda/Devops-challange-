# TASK-2: EC2 + CloudWatch Alarm + SNS Notification (TechMart)

## 📌 Objective
Deploy a simple website ("TechMart - 24/7 Electronics Shopping") on an
EC2 instance, and build a monitoring + alerting pipeline that emails
me when CPU usage exceeds 80%.

---

## 🗂️ Project Structure
TASK-2-CloudWatch-SNS/
├── index.html      # TechMart static webpage served via Apache on EC2
└── README.md         # Documentation for this task

---

## 🛠️ Tools & Services Used
- **AWS EC2** (Amazon Linux 2023, t2.micro)
- **Apache HTTP Server (httpd)**
- **AWS CloudWatch** (Alarms)
- **AWS SNS** (Simple Notification Service)

---

## 🚀 Steps Performed

### 1. Website
Created `index.html` for TechMart and hosted it via Apache on EC2.

### 2. EC2 Instance
- AMI: Amazon Linux 2023
- Instance type: t2.micro (free tier)
- Security Group: SSH (22) from my IP, HTTP (80) from anywhere
- User data script auto-installed and started `httpd` on boot

### 3. SNS Topic
- Created Standard topic: `TechMart-CPU-Alerts`
- Subscribed my email and confirmed the subscription

### 4. CloudWatch Alarm
- Metric: `CPUUtilization` (per-instance)
- Statistic: Average
- Period: 5 minutes
- Threshold: > 80%
- Datapoints to alarm: 1 out of 1
- Action: Notify `TechMart-CPU-Alerts` SNS topic when in ALARM state

### 5. Testing
Simulated high CPU using:
```bash
yes > /dev/null &
yes > /dev/null &
```
Alarm triggered within ~5–10 minutes, email alert received successfully.
Stopped load with:
```bash
pkill yes
```

---

## 🌐 Live Website
http://<EC2-PUBLIC-IP>
---

## 🎯 Key SRE Concepts Learned

| Concept | Why it matters |
|---|---|
| CloudWatch Alarms | Metric + threshold + evaluation window = core monitoring pattern |
| Datapoints to alarm | Controls alert sensitivity vs alert fatigue |
| SNS | Pub/sub notification backbone, extendable to Slack/PagerDuty |
| Security Groups | Stateful virtual firewall for EC2 |
| User Data Scripts | First step toward infrastructure automation |
| Load testing | Validating alerting pipelines actually work before relying on them |

---

## 🔜 Next Steps (Additional Tasks)
- **TASK-3:** Auto-remediation using Lambda triggered by the same alarm
- **TASK-4:** Replace manual EC2 setup with Terraform
- **TASK-5:** Add CloudWatch dashboard for visual monitoring

---

# 📘 End-to-End DevOps Platform – Project Documentation

## 📌 Overview

This repository is the **central documentation and control hub** for an end-to-end DevOps project built using **multiple repositories** and **enterprise-style practices**.

The project demonstrates:

- Clear separation of concerns
- Centralized CI/CD using Jenkins
- Infrastructure as Code and deployments
- Monitoring, logging, and alerting
- Slack-based approvals and notifications
- Disaster Recovery (DR) readiness
- One-command start/stop of the entire platform

> **Core idea:**  
> Application code, pipelines, infrastructure, monitoring, and documentation are decoupled but unified through a controlled and auditable workflow.

---

## 🗂️ Repository Architecture

| Repository                                 | Description                                | Link                                                              |
| ------------------------------------------ | ------------------------------------------ | ----------------------------------------------------------------- |
| **frontend-app-repo**                      | Frontend application source code           |https://github.com/AkshatJoshi-18/snapcart-frontend              |
| **backend-app-repo**                       | Backend application source code            | https://github.com/AkshatJoshi-18/snapcart-backend               |
| **jenkins-pipeline-repo**                  | Centralized Jenkins pipeline code          | https://github.com/AkshatJoshi-18/snapcart-jenkins-pipeline          |
| **infrastructure-deployment-repo**         | Infrastructure as Code & deployments       | https://github.com/AkshatJoshi-18/snapcart-infrastructure-and-deployment |
| **monitoring-logging-repo**                | Monitoring and logging configuration       | https://github.com/AkshatJoshi-18/snapcart-monitoring-logging        |
| **documentation-control-repo (this repo)** | Documentation + start/stop control scripts | https://github.com/AkshatJoshi-18/snapcart-documentation-and-scripts     |

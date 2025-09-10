# 📅 Day 11 - Install and Configure Tomcat Server

**Challenge**: [KodeKloud 100 Days of DevOps](https://kodekloud.com/)

## ✅ Task Summary

The goal for Day 11 is to install, configure, and deploy a Java-based application using **Apache Tomcat** on **App Server 2 (stapp02)**.

### 📝 Requirements

- Install **Tomcat** on `stapp02`
- Configure it to run on port `8084`
- Deploy a `ROOT.war` file from the **Jump Host** located at `/tmp/ROOT.war`
- Verify the application is accessible via:
  ```bash
  curl http://stapp02:8084

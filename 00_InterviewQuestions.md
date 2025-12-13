
## **1. What role does GitHub Actions play in your CI/CD pipeline?**

**Answer:**
GitHub Actions automates the complete CI/CD lifecycle, including source code checkout, build, test, Docker image creation, security scanning, pushing images to Azure Container Registry (ACR), and deploying applications to Azure Kubernetes Service (AKS).

---

## **2. How does your CI pipeline start and what triggers it?**

**Answer:**
The CI pipeline is triggered on `push` and `pull_request` events to specific branches (such as `main` or `dev`). This ensures every code change is automatically built, tested, and validated before deployment.

---

## **3. Why did you push Docker images to Azure Container Registry (ACR)?**

**Answer:**
ACR is a private, secure container registry tightly integrated with Azure services. It allows AKS to pull images securely and efficiently, supports role-based access control, and improves deployment reliability.

---

## **4. How did you authenticate GitHub Actions with Azure?**

**Answer:**
Authentication was done using **Azure credentials stored as GitHub Secrets** (Service Principal or OIDC). GitHub Actions uses `azure/login` to securely authenticate without exposing credentials in the workflow.

---

## **5. Why did you use a LoadBalancer service for the web application in AKS?**

**Answer:**
A LoadBalancer service exposes the web application to the internet by assigning a public IP address, allowing external users to access the frontend.

---

## **6. Why did you use a ClusterIP service for the backend?**

**Answer:**
ClusterIP restricts backend access to within the Kubernetes cluster, improving security by preventing direct external access and allowing communication only from trusted internal services like the frontend.

---

## **7. How are secrets managed in your GitHub Actions pipeline?**

**Answer:**
Sensitive values such as Azure credentials, ACR login details, and AKS configuration are stored in **GitHub Secrets** and referenced securely in workflows using `${{ secrets.NAME }}`.

---

## **8. Why is secrets management important in CI/CD pipelines?**

**Answer:**
Secrets management prevents credential leaks, protects cloud resources, and ensures compliance with security best practices by keeping sensitive data out of source code and logs.

---

## **9. What is Trivy and how did you use it in your pipeline?**

**Answer:**
Trivy is a container security scanner used to detect vulnerabilities in Docker images. In the pipeline, Trivy scans the image after build and fails the workflow if critical vulnerabilities are found.

---

## **10. Why is container image scanning important before deployment?**

**Answer:**
Image scanning ensures that known vulnerabilities, misconfigurations, and security risks are identified early, preventing insecure containers from being deployed to production environments.

---

## **11. What is Blue-Green deployment and why did you use it in AKS?**

**Answer:**
Blue-Green deployment runs two identical environments (blue and green). Traffic is switched to the new version only after validation, enabling **zero-downtime deployments** and instant rollback if issues occur.

---

## **12. How do you switch traffic between Blue and Green deployments?**

**Answer:**
Traffic is switched by updating the Kubernetes Service selector to point from the old deployment (blue) to the new deployment (green), without restarting or affecting users.

---

## **13. How does rollback work in your AKS deployment?**

**Answer:**
Rollback is achieved by reverting the Service selector back to the previous deployment or using `kubectl rollout undo`, instantly restoring traffic to the stable version if the new deployment fails.

---

## **14. What is the difference between Rolling Update and Blue-Green deployment?**

**Answer:**

* **Rolling Update:** Gradually replaces pods, which may temporarily affect users.
* **Blue-Green:** Deploys a full new version separately and switches traffic instantly, offering safer releases and faster rollback.

---

## **15. How does GitHub Actions ensure deployment reliability in your project?**

**Answer:**
Reliability is ensured through automated testing, image scanning with Trivy, controlled deployments using Blue-Green strategy, secure secrets management, and rollback mechanisms in AKS.

---

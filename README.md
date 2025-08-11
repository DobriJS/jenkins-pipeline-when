# Jenkins Pipeline: Deploy Web Application with Apache or Nginx in Docker

This repository contains a Jenkins pipeline that automates the deployment of a web application to either **Apache HTTPD** or **Nginx** running inside Docker.  
The pipeline uses an environment variable to choose the web server type and manages the container lifecycle, including cleaning up old deployments.

---

## 🚀 Pipeline Overview

### **Environment Variable**
- `WEBSERVER` — determines which web server to deploy:
  - `"Apache"` → Apache HTTPD container
  - `"Nginx"` → Nginx container  
  *(Default: `Nginx`)*

---

### **Pipeline Stages**

1. **Create Directory for the Web Application**
   - Removes any existing `/home/jenkins/app-web` directory.
   - Creates a fresh directory for deployment.

2. **Drop the Container**
   - Removes the existing `app-web` Docker container (if it exists).

3. **Create the Apache Container** *(Conditional)*
   - Runs if `WEBSERVER=Apache`.
   - Maps `/home/jenkins/app-web` to `/usr/local/apache2/htdocs/`.
   - Exposes the container on port `9100`.

4. **Create the Nginx Container** *(Conditional)*
   - Runs if `WEBSERVER=Nginx`.
   - Maps `/home/jenkins/app-web` to `/usr/share/nginx/html`.
   - Exposes the container on port `9100`.

5. **Copy the Web Application**
   - Copies files from the `web/` directory in the Jenkins workspace to `/home/jenkins/app-web`.

---

### **Post Actions**
- **Always** — Displays a message indicating post-build steps are executed.
- **On Success** — Prints confirmation, archives web files, and cleans the Jenkins workspace.
- **On Failure** — Prints an error message.

---

## 🐳 Requirements
- **Jenkins** installed with Docker support.
- **Docker** engine running on the Jenkins agent.
- Apache HTTPD (`httpd`) and Nginx (`nginx`) Docker images.
- A `web/` directory containing your application files in the Jenkins workspace.

---

## 📌 Usage
1. Place your web application files in the `web/` folder.
2. Update the `WEBSERVER` environment variable in the pipeline:
   ```groovy
   environment {
       WEBSERVER = "Nginx" // or "Apache"
   }

# SonarQube Setup & Project Analysis Guide

This document provides step-by-step instructions for running **SonarQube** in Docker, setting up a project, generating tokens, and running analysis.

---

## 🚀 Prerequisites
- Docker installed and running  
- Sonar-scanner-cli
- PowerShell (for Windows users)  
- A local project you want to analyze  

---

## ⚙️ Setup Instructions

### ** Run Docker Compose**
dotnet tool install --global dotnet-sonarscanner

### Incase dotnet-sonarscanner throw 401 erro follow below instructions 

   ## Run this command
   dotnet nuget list source

   ## Disable private nugget temporarily
   dotnet nuget enable source embrace-it

   ### Run command again (sonnarscanner)

   ### Enable the packages againg

---

### **Step 1: Run Docker Compose**
```bash
docker compose -f Sonar.yml up
```

---

### **Step 2: Run SonarQube Docker Image**
```bash
docker run -d --name sonarqube -p 9000:9000 sonarqube:latest
```

SonarQube will be accessible at:  
👉 [http://localhost:9000](http://localhost:9000)

---

### **Step 3: Login to SonarQube**
- **Default Username:** `admin`  
- **Default Password:** `admin`  

On first login, you’ll be prompted to update your password.

---

### **Step 4: Setup a Project**
1. Navigate to **Projects** in the SonarQube dashboard.  
2. Click **Create Project**.  
3. Select **Local Project** and configure the required details.  

---

### **Step 5: Generate a Token**
1. Go to **Administration → Projects → Management**.  
2. Click on your project name.  
3. Select **Locally**.  
4. Click **Generate Project Token**.  
5. Save this token securely — it will be required for analysis.  

---

### **Step 6: Run Analysis Command**

### *** To run analysis in visual studio

   # 1. Project Build
   dotnet build
   dotnet test --no-build

   # 2. Code Coverage
   dotnet sonarscanner begin /k:"PROJECTTOKENKEY" /d:sonar.host.url="http://localhost:9000"  /d:sonar.token="TOKEN"

   # 2. Build the solution
    dotnet build 

   # 3. Run tests with coverage (make sure you have coverlet.collector installed in test projects)
      dotnet test D:/src/xyz.sln /p:CollectCoverage=true /p:CoverletOutput=TestResults /p:CoverletOutputFormat=opencover


## ✅ Verification
After running the analysis, refresh your SonarQube dashboard.  
You should see code quality metrics, bugs, vulnerabilities, and other reports for your project.

---

## 📌 Notes
- Ensure port `9000` is not in use before running SonarQube.  
- Tokens are sensitive; do not share them publicly.  
- If SonarQube doesn’t start, check Docker logs:
  ```bash
  docker logs sonarqube
  ```

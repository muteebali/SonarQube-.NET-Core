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

### ** Step 1: Sonar Scanner **
```bash
dotnet tool install --global dotnet-sonarscanner --source https://api.nuget.org/v3/index.json
```

### **Step 2: Run Docker Compose**
```bash
docker compose -f ./Sonar.yml down -v
docker compose -f Sonar.yml up
```

---

### **Step 3: Run SonarQube Docker Image**
Sonar Qube Container will be up and running at [localhost://9000](http://localhost:9000)

### **Step 4: Login to SonarQube**
- **Default Username:** `admin`  
- **Default Password:** `admin`  

On first login, you’ll be prompted to update your password.

---

### **Step 5: Setup a Project**
1. Navigate to **Projects** in the SonarQube dashboard.  
2. Click **Create Project**.  
3. Select **Local Project** and configure the required details.  

---

### **Step 6: Generate a Token**
1. Go to **Administration → Projects → Management**.  
2. Click on your project name.  
3. Select **Locally**.  
4. Click **Generate Project Token**.  
5. Save this token securely — it will be required for analysis.  

---

### **Step 7: Run Analysis Command**

### ***Open Visual Studio Developer Power Shel** 
   #### 1. Clear Test Files
   ```bash
   Get-ChildItem -Path . -Recurse -Directory -Filter "TestResults" | Remove-Item -Recurse -Force
   ```

   #### 2. Sonar Build 
   ```bash
    dotnet sonarscanner begin /k:"GLS.DataPipeline" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="sqp_19212f5c5b89dbde7b4e0a2ae94be75fca590b5b" /d:sonar.cs.opencover.reportsPaths="**/TestResults/coverage.opencover.xml"

   ```

#### 3. Build the solution
   ```bash
      dotnet build --no-incremental
   ```

   ### 4. Test Coverage for each project
   ```bash
      dotnet test --no-build /p:CollectCoverage=true /p:CoverletOutput=TestResults/coverage.opencover.xml /p:CoverletOutputFormat=opencover
   ```
   
   #### 5. Check Code Coverage FIle
   ```bash
   Get-ChildItem -Recurse -Filter coverage.opencover.xml
   ```

   #### 6. Finishing
   ```bash
      dotnet sonarscanner end /d:sonar.login="sqp_19212f5c5b89dbde7b4e0a2ae94be75fca590b5b"
   ```


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



### To verify the coverlet library in projects
Get-ChildItem -Recurse -Filter *.csproj | ForEach-Object { Write-Host "Checking $_"dotnet list $_ full | Select-String "coverlet"}

### If not installed
Get-ChildItem -Recurse -Filter *.csproj | Where-Object { $_.FullName -like "*Tests*" } | ForEach-Object {dotnet add $_.FullName package coverlet.collector --source https://api.nuget.org/v3/index.json}
Get-ChildItem -Recurse -Filter *.csproj | Where-Object { $_.FullName -like "*Tests*" } | ForEach-Object {dotnet add $_.FullName package coverlet.msbuild --source https://api.nuget.org/v3/index.json}

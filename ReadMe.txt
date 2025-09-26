//Step : 1 run the command
Docker compose -f Sonar.yml -d

//Step : 2 Run Docker Image
docker run -d --name sonarqube -p 9000:9000 sonarqube:latest

//Step : 3 Login 
Default User: admin
Default Password: admin

Redirect to change password screen update the password


//Step: 4 Setup Project
Create Local Project 

//Step: 5 Get Token
Goto Administration Tab and click on Project and then Management and then click on project name and then click on locally and the generate a prokect token button

//Step: 6 Run Analysi Command
Open the AnalysisCommand powershell file and do the changes and run it in powershell
powershell -ExecutionPolicy Bypass -File .\AnalysisCommand.ps1
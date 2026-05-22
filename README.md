# java-spring-boot-project
i have created one java project from "https://start.spring.io/" this website to test the complete workflow from jenkins like, building the code, pushing artifact to nexus repo, bilding docker image, pushing docker image to nexus repo, do code quality check using sonar qube, and deploy onto server using docker......


### 100% working #####

code is working 100 % working correctly at this comit 


### pre-requisets
1. java 17 (add path "/usr/lib/jvm/java-17-openjdk-amd64") --> store in tools in jenkins

2. maven 3 (auto install in jenkins with name "maven3" because same name pipline )


### In jenkins
pluugins to install
1. sonar qube scanner
2. docker pipeline (not neessary)

store token and credential to jenkins

1. Nexus username and password

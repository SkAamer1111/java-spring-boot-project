// sonar sccanner is failing i need to see why, i think i used skaamer1111 organization
// but projectKey should be in the same organization, 

pipeline{
    
    agent any
    
    triggers {
        githubPush()
    }

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    environment {
        PROJECT_NAME = 'java-app'
        PROJECT_KEY = 'java-app'
        ORGANIZATION_KEY = 'skaamer1111'
        HOST_URL = 'https://sonarcloud.io'
        IMAGE_NAME = 'shaikhaamer/spring-boot'
    }

    stages{
        stage ("CODE") {
        steps{

            git url: 'https://github.com/SkAamer1111/java-spring-boot-project.git ', branch: 'main'            
            }
        }
        stage ('CODE BUILD'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage ('CODE QUALITY'){
            steps{
                withSonarQubeEnv ('sonar-server'){
                    sh "mvn sonar:sonar -Dsonar.projectKey:$PROJECT_KEY -Dsonar.projectName:$PROJECT_NAME -Dsonar.organization=$ORGANIZATION_KEY "
                }
            }
        }
        stage ('ARCHIVE'){
            steps{
                archiveArtifacts artifacts: 'target/*.jar' 
            }
        }
        stage ('TEST_CODE'){
            steps{
                sh 'mvn test'
            }
        }
        stage ('BUILD-IMAGE'){
            steps{
                sh 'docker image build -t $IMAGE_NAME:1.0 .'
            }
        }
        stage ('DEPLOY-APP'){
            steps {
                sh 'docker container run -d --name deploy -P $IMAGE_NAME:1.0'
            }
        }
    }
}

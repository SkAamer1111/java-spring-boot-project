pipeline{
    
    agent any
    
    triggers {
        githubPush()
    }

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }
    environment{
        POJECT_NAME : 'java-app'
        PROJECT_KEY : 'java-app'
        ORGANIZATION_KEY : 'gameoflife-project'
        HOST_URL : 'https://sonarcloud.io'
        IMAGE_NAME : 'shaikhaamer/spring-boot'
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
                    sh 'mvn sonar:sonar -Dsonar.projectKey:$PROJECT_KEY -Dsonar.projectName:$POJECT_NAME -Dsonar.organization=$ORGANIZATION_KEY -Dsonar.host.url=$HOST_URL'
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

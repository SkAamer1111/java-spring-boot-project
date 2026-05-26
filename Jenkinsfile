// I am using Nexus artifact uploader to push Artifact to nexus repo,
// but i am working on killer coda, so http request are dailing , 
// thats why artifact not pushing to nexus 

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
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=$PROJECT_KEY -Dsonar.projectName=$PROJECT_NAME -Dsonar.organization=$ORGANIZATION_KEY "
                }
            }
        }
        stage ('ARCHIVE'){
            steps{
                archiveArtifacts artifacts: 'target/*.jar' 
            }
        }
        stage('PUSH ARTIFCT'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'java-app', classifier: '', file: 'target/demo-0.0.1-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexus-pass', groupId: 'java-app', nexusUrl: 'e45794d1eedb-10-244-3-247-8081.papa.r.killercoda.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-new', version: '1.0'
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
        stage ('PUSH IMAGE TO NEXUS'){
            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-pass',
                        usernameVariable: 'USERNAME',
                        passwordVariable: 'PASSWORD' 
                    )
                ]) {
                    sh """ echo $PASSWORD | docker login 199195c91dd3-10-244-4-181-8081.papa.r.killercoda.com:8083 -u $USERNAME --password-stdin """
                    sh """ docker tag $IMAGE_NAME:1.0 199195c91dd3-10-244-4-181-8081.papa.r.killercoda.com:8083/$IMAGE_NAME:1.0 """
                    sh """ docker push 199195c91dd3-10-244-4-181-8081.papa.r.killercoda.com:8083/$IMAGE_NAME:1.0 """
                        
                }
            }
        }
        stage ('DEPLOY-APP'){
            steps {
                sh 'docker container run -d --name deploy -P $IMAGE_NAME:1.0'
            }
        }
    }
}

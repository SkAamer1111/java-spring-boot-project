// showing error UNAUTHORISED ERROR while pushing artifat to repository"
// i have did some changes , need to check, i missed 

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
        stage ('PUSH ARTIFACT TO NEXUS'){
            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-pass',
                        usernameVariable: 'USERNAME',
                        passwordVariable: 'PASSWORD' 
                    )
                ]) {
                    sh """
                    mvn deploy:deploy-file \
                        -Dfile=target/demo-0.0.1-SNAPSHOT.jar  \
                        -Durl=https://d4ca1922d634-10-244-6-175-8081.spca.r.killercoda.com/repository/maven-releases-new/ \
                        -DgroupId=java-app \
                        -Dversion=1.0 \
                        -DrepositoryId=nexus \
                        -DartifactId=java-app \
                        -Dpackaging=jar \
                        -Dusername=$USERNAME \
                        -Dpassword=$PASSWORD
                    """
                        
                }
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
                    sh """echo $PASSWORD | docker login da54b478c7fb-10-244-6-161-8080.papa.r.killercoda.com:8083 -u $USERNAME --pasword-stdin \
                          docker tag $IMAGE_NAME:1.0 da54b478c7fb-10-244-6-161-8080.papa.r.killercoda.com:8083/$IMAGE_NAME:1.0 \
                          docker push 463827724f12-10-244-3-48-8081.papa.r.killercoda.com:8083/$IMAGE_NAME:1.0"""
                        
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

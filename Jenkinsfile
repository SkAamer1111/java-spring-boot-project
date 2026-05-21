pipeline{
    
    aagent any
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
        stage ('TEST_CODE'){
            steps{
                sh 'mvn test'
            }
        }
        stage ('BUILD-IMAGE'){
            steps{
                sh 'docker image build -t java-image .'
            }
        }
        stage ('DEPLOY-APP'){
            steps {
                sh 'docker container run -d --name deploy -P java-image'
            }
        }
    }
}

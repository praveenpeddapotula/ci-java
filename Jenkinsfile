pipeline {
    agent any
    tools {
        maven 'MAVEN3.9.9'
        jdk 'JDK9'

    }
    stages {
        stage('checkout'){
            steps{
                git url:'https://github.com/praveenpeddapotula/ci-java.git' , branch: 'main'
            }
        }
        stage('build'){
            steps{
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('unit test'){
            steps{
                sh 'mvn test'
            }
        
        post {
            always {
                junit '**/target/surefire-reports/*.xml'
            }
            unsuccessful {
                echo 'unit tests failed ,stopping the pipeline'
            }
        }
        }
        stage('integration testing') {
            steps {
                sh 'mvn verify -P integration-tests'
            }
        
        post {
            always{
                junit '**/target/failsafe-reports/*.xml'
            }
            unsuccessful {
                echo 'integration tests failed stopping the pipeline'
            }
        }
        }
        stage('package'){
            steps{
                sh 'mvn package'
            }
            post {
                success {
                        archiveArtifacts artifacts:'**/target/*.jar' ,allowEmptyArchive: false                }
            }
        }
    }
}
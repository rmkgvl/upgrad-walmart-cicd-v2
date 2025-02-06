pipeline {
    agent any

    tools {
        maven "maven-3.9.9"
    }

    stages {
        
        stage('Source'){
            steps {
               echo 'Checking out source code from GitHub' 
               checkout scmGit(branches: [[name: '*/main']], extensions: [cleanBeforeCheckout()], userRemoteConfigs: [[credentialsId: 'github-cred', url: 'https://github.com/ramanujds/upgrad-wmart-cicd-v2']])
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests'
                dir('my-spring-boot-app') {
                   sh "mvn test"
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application'
                dir('my-spring-boot-app') {
                   sh "mvn clean package"
                }
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'my-spring-boot-app/target/*.jar'
                }
            }
        }
    }
}

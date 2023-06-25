pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ken4class/testrepo'
            }
        }
        
        // stage("SonarQube analysis") {
        //    steps {
        //      withSonarQubeEnv('sonar') {
        //          sh 'mvn clean package sonar:sonar'
        //      }
        //    }
        // }
        
        // stage('Quality Gate') {
        //     steps {
        //         waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
        //     }
        // }
        
        
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'RegistrationApp',
               classifier: '', file: 'target/RegistrationApp-1.3.war',
               type: 'war']], 
               credentialsId: 'nexus', 
               groupId: 'com.example', 
               nexusUrl: '3.84.27.169:8081', 
               nexusVersion: 'nexus3', 
               protocol: 'http', 
               repository: 'my-repo',
               version: '1.4'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
               echo 'Deploy to Tomcat'
            }
        }
        
        stage('Publish Test Result') {
            steps {
                junit 'target/surefire-reports/TEST-com.example.mywebapp.RegisterServletTest.xml'
            }
        }
    }
}
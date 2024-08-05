pipeline {
    agent any

    tools {
        jdk 'java'
        maven 'Maven'
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'git-creds', poll: false, url: 'https://github.com/gopal99489/gctech.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('SonarQube analysis') {
            environment {
                SONAR_HOST_URL = 'http://34.100.152.179:9000'
                SONAR_TOKEN = 'sqp_a8a95ab040733fa92b550d62ce3fdbd6af5094ff'
            }
            steps {
                sh '''
                sudo docker run --rm \
                -e SONAR_HOST_URL=$SONAR_HOST_URL \
                -e SONAR_TOKEN=$SONAR_TOKEN \
                -v "$(pwd):/usr/src" \
                sonarsource/sonar-scanner-cli \
                -Dsonar.projectKey=gc-tech \
                -Dsonar.sources=src \
                -Dsonar.java.binaries=target/classes \
                -Dsonar.java.libraries=target/*.war \
                -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
         stage('Deploy to Tomcat') {
            steps {
                sh 'sudo cp /var/lib/jenkins/workspace/gopal/target/vamsi.war /opt/tomcats/tomcat10/webapps/'
            }
        }
    }
}

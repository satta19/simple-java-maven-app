pipeline {
    agent any
        tools {
        maven "Maven"
    }
    stages {
        stage('Git Pull'){
            steps {
            checkout scm
        }
        }
        stage('Maven Build'){
            steps {
                sh "mvn clean install"
                sh "mvn -B package"
            }
        }
        stage('Sonarqube') {
    environment {
        scannerHome = tool 'sonarqube_scanner'
        SONAR_PROJECT_NAME = "sonar_jenkins-pipeline-as-develop"
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh '''${scannerHome}/bin/sonar-scanner \
            -Dsonar.java.binaries=target/classes/com/mycompany/app \
            -Dsonar.projectKey=$SONAR_PROJECT_NAME \
            -Dsonar.sourceEncoding=UTF-8 \
            -Dsonar.sources=.'''
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
    }
}

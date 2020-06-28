pipeline {
    agent any
    
    stages {
        stage('Git Pull'){
            steps {
            checkout scm
        }
        }
        stage('Sonarqube') {
    environment {
        scannerHome = tool 'sonarqube_scanner'
        PROJECT_NAME = "sonar_jenkins-pipeline-as-code"
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh '''${scannerHome}/bin/sonar-scanner \
            -Dsonar.java.binaries=build/classes/java/ \
            -Dsonar.projectKey=$PROJECT_NAME \
            -Dsonar.sources=.'''
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
    }
}

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
        PROJECT_NAME = "sonar_jenkins-pipeline-as-develop"
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh '''${scannerHome}/bin/sonar-scanner \
            -Dsonar.java.binaries=target/classes/com/mycompany/app \
            -Dsonar.projectKey=$PROJECT_NAME \
            -Dsonar.sonar.branch.name=develop \
            -Dsonar.sources=.'''
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
    }
}

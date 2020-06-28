pipeline {
    agent any
    
    stages {
        stage('Git Pull'){
            checkout scm
        }
        
        stage('Sonarqube') {
    environment {
        scannerHome = tool 'sonarqube_scanner'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
    }
}

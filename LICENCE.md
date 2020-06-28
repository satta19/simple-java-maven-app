Hello World !!
**Welcome to Dev**
----------------
pipeline {
    agent any
        tools {
        maven "Maven"
    }
    stages {
        stage('Git Pull'){
            steps {
                script {
                    echo "$env.branch_name"
                    echo "$env.project_name"
            git branch: "$env.branch_name", credentialsId: 'github', url: "https://github.com/satta19/$env.project_name"
                }
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
        SONAR_PROJECT_NAME = "${env.project_name}_delivery"
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


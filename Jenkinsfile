pipeline {
    stages {
        stage('Checkout') {
            checkout(
                [$class: 'GitSCM', branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                submoduleCfg: [], 
                userRemoteConfigs: [[url: 'https://github.com/andregoncrod/flatris-pd']]]
            ) 
        }

        stage('Copy and rename env file') {
            steps {
                sh 'cp .env.example.js .env.js'
            }
        }

        stage('Build') {
            environment {
                NODE_VERSION = '12'
            }
            steps {
                withNodeJS(version: NODE_VERSION) {
                sh 'npm ci'
                sh 'npm run build'
                }
            }
        }

        stage('Upload Artifact') {
            steps {
                archiveArtifacts artifacts: 'web/.next', fingerprint: true
            }
        }
    }
}
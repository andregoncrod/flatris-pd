node {
    stage('Checkout') {
        checkout(
            [$class: 'GitSCM', 
            branches: [[name: 'master']], 
            userRemoteConfigs: [[url: 'https://github.com/andregoncrod/flatris-pd']]]) 
    }

    stage('Copy and rename env file') {
        sh 'cp .env.example.js .env.js'
    }

    stage('Build') {
        nodejs(nodeJSInstallationName: 'nodejs12') {
            sh 'npm ci'
            sh 'npm run build'
        }
    }

    stage('Upload Artifact') {
        archiveArtifacts artifacts: 'web/.next/**/*', fingerprint: true
    }
}
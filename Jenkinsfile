node {
    stage('Checkout') {
        def commit_id
        checkout(
            [$class: 'GitSCM', 
            branches: [[name: 'master']], 
            userRemoteConfigs: [[url: 'https://github.com/andregoncrod/flatris-pd']]])    
        sh "git rev-parse --short HEAD > .git/commit-id"                        
        commit_id = readFile('.git/commit-id').trim()
    }

    stage('Copy and rename env file') {
        sh 'cp .env.example.js .env.js'
    }

    stage('Test / Build') {
        nodejs(nodeJSInstallationName: 'nodejs12') {
            sh 'npm ci'
            sh 'npm test'
            sh 'npm run build'
        }
    }

    stage('Upload Artifact') {
        archiveArtifacts artifacts: 'web/.next/**/*', fingerprint: true
    }

    stage('Docker Build / Push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
       def app = docker.build("andregoncrod/flatris-pd-webapp:${commit_id}", '.').push()
     }
   }
}
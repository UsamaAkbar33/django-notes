@Library("Shared") _
pipeline {
    agent { label "Agnet-2nd" }

    stages {
        
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }

        stage('Code') {
            steps {
                echo 'This is cloning the code'
                git url: 'https://github.com/UsamaAkbar33/django-notes-app.git', branch: 'main'
                                echo 'Code cloning successful'
            }
        }

        stage('Build') {
    steps {
        sh 'docker build -t usamashams/notes-app:latest .'
    }
}

stage('Push to DockerHub') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhubcred',
                usernameVariable: 'dockerHubUser',
                passwordVariable: 'dockerHubPass'
            )
        ]) {
            sh '''
                echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                docker push $dockerHubUser/notes-app:latest
            '''
        }
    }
}
        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                sh 'docker compose down && docker compose up -d'
            }
        }
    }
}

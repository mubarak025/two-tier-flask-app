pipeline {
    agent { label "prod"}

    stages {
        stage('Code Clone') {
            steps {
                git url: 'https://github.com/mubarak025/two-tier-flask-app.git',
                    branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the application...'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'dockerHubUser',
                        passwordVariable: 'dockerHubPass'
                    )
                ]) {
                    sh '''
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin

                        docker tag flask-app "$dockerHubUser/flask-app:latest"

                        docker push "$dockerHubUser/flask-app:latest"

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build flask-app'
            }
        }
    }
}

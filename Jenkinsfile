pipeline {
    agent {
        label "vinod"
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning the code...'

                git branch: 'main',
                    url: 'https://github.com/aniket01299/django-notes-app.git'

                echo 'Code cloned successfully'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                
                sh 'docker build -t notes-app:latest .'
               
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'This is Pushing the image to Docker Hub'
               withCredentials([usernamePassword(
    credentialsId: 'dockerHubCred',
    usernameVariable: 'dockerHubUser',
    passwordVariable: 'dockerHubPass'
)]) {

    sh '''
    echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
    docker tag notes-app:latest $dockerHubUser/notes-app:latest
    docker push $dockerHubUser/notes-app:latest
    '''
}
            }
        }

        stage('Deploy') {
            steps {
                 sh '''
                  docker-compose down || true
                  docker-compose up -d --build
                  '''
            }
        }
    }
}

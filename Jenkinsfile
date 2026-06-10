pipeline {
    agent any

    stages {

        stage('Build Docker image') {
            steps {
                sh 'docker build -t flask_app .'
            }
        }



        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker image to DockerHub') {
            steps {
                sh 'docker tag flask_app hit52/flask_app:latest'
                sh 'docker push hit52/flask_app:latest'
            }
        }
      stage('Deploy')  {
         steps{
             sh 'docker stop flaskcont || true'
             sh 'docker rm flaskcont || true'
             sh 'docker run -d -p 5000:5000 --name flaskcont hit52/flask_app:latest'
        }
      
      }

}        






    }
}

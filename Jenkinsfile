pipeline {
    agent { 
        node {
            label 'docker-agent-java'
      }
    }
    stages {
        stage('Pull source') {
            steps {
                echo "Pull source code.."
                git branch: 'main', url: 'https://github.com/tranthinh222/gradle-ci-cd-demo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image.."
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                echo 'Push Docker Image....'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', 
                                          usernameVariable: 'DOCKER_USER', 
                                          passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker tag myapp:latest tranthinh123/myapp:latest
                    docker push tranthinh123/myapp:latest
                    '''
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d --name myapp -p 8081:8081 tranthinh123/myapp:latest
                '''
            }
        }
    }
}
pipeline {
    tools {
        jdk 'java17'
        maven 'Maven'
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Pulling from GITHUB repository"
                git branch: 'main',
                    credentialsId: 'mygithubcred',
                    url: 'https://github.com/premkumar-490/two.git'
            }
        }

        stage('Test the Project') {
            steps {
                echo "Test my JAVA project"
                bat 'mvn clean test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    echo 'Test Run succeeded!'
                }
            }
        }

        stage('Build Project') {
            steps {
                echo "Building my JAVA project"
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build the Docker Image') {
            steps {
                echo "Build the Docker Image for mvn project"
                bat 'docker build -t mvnproj:1.0 .'
            }
        }

         stage('Push Docker Image to DockerHub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhubpwd',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat '''
                    docker logout
                    echo %DOCKER_PASS%| docker login -u %DOCKER_USER% --password-stdin
                    docker tag mvnproj:1.0 %DOCKER_USER%/myapp:latest
                    docker push %DOCKER_USER%/myapp:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                echo "Running Docker container"
                bat '''
                docker rm -f myjavaapp || exit 0
                docker run --name myjavaapp priyanrk17/myapp:latest
                '''
            }
        }
    }


    post {
        success {
            echo 'I succeeded!'
        }
        failure {
            echo 'Failed........'
        }
    }
}

tools {
jdk 'java17'
maven 'Maven'
}

stages {

stage('Checkout Code') {
steps {
git branch: 'main',
credentialsId: 'mygithubcred',
url: 'https://github.com/premkumar-490/two.git'
}
}

stage('Test Project') {
steps {
bat 'mvn clean test'
}
post {
always {
junit '**/target/surefire-reports/*.xml'
}
}
}

stage('Build Project') {
steps {
bat 'mvn clean package -DskipTests'
}
}

stage('Build Docker Image') {
steps {
bat 'docker build -t mymvnproj:latest .'
}
}

stage('Push Docker Image') {
steps {
withCredentials([usernamePassword(
credentialsId: 'dockerhubpwd',
usernameVariable: 'DOCKER_USER',
passwordVariable: 'DOCKER_PASS'
)]) {
bat '''
echo %DOCKER_PASS%| docker login -u %DOCKER_USER% --password-stdin
docker tag mymvnproj:latest %DOCKER_USER%/mymvnproj:latest
docker push %DOCKER_USER%/mymvnproj:latest
'''
}
}
}

stage('Deploy to Kubernetes') {
steps {
bat '''
"C:\\Minikube\\minikube.exe" start
kubectl apply -f deployment.yaml
timeout /t 20
kubectl get pods
kubectl apply -f services.yaml
kubectl get svc
'''
}
}

stage('Parallel Operations') {
parallel {

stage('Enable Metrics') {
steps {
bat '"C:\\Minikube\\minikube.exe" addons enable metrics-server'
}
}

stage('Verify Services') {
steps {
bat 'kubectl get svc'
}
}
}
}
}

post {
success {
echo 'Pipeline executed successfully 🚀'
}
failure {
echo 'Pipeline failed ❌'
}
}
}
>>>>>>> 5547ca5 (1)

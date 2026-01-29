pipeline {
    agent any
    tools{
         jdk 'java17'
         maven 'Maven'
    }
    stages {
        stage('Checkout Code') {
            steps {
               echo "Pulling from GITHUB repository"
               git branch: 'main', credentialsId: 'mygithubcred', url: 'https://github.com/premkumar-490/two.git'
            }
        }
        stage('Build Project') {
            steps {
               echo "Building my JAVA project"
               bat 'mvn clean package' 
            }
        }
        stage('Test The Appln') {
            steps {
               echo "Testing my JAVA project"
            }
        }
        stage('Deploy the project') {
            steps {
                echo "Project is getting Deployed"
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

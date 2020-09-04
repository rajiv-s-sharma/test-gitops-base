pipeline {
    agent any
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!'
                dir('k8s-configuration') {
                  sh 'ls -l'
                }

            }
        }
    }
}
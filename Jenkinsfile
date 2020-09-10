pipeline {
  parameters {
        booleanParam(name: 'SKIP_SECURITY', defaultValue: false, description: 'Skip Security Gates')
    }
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
        stage('Stage 2') {
          when {
            allOf {
              branch 'master';
              expression {SKIP_SECURITY.toBoolean()}
            }
          }
          steps {
            echo 'Stage 2'
          }
        }
    }
}
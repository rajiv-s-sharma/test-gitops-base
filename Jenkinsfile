pipeline {
  environment {
    SOURCE_CODE_REPO = 'https://github.wdf.sap.corp/sfsf-platform-core/uxrhp-cardsvc.git'
  }
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
          expression {params.SKIP_SECURITY}
        }
      }
      steps {
        def gitBranchUrlWithCommitId = "https://${SOURCE_CODE_REPO}@${BRANCH_NAME}:${GIT_COMMIT}"
        echo 'Stage 2'
        echo gitBranchUrlWithCommitId
      }
    }
  }
}
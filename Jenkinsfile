pipeline {
  environment {
    SOURCE_CODE_REPO = 'https://github.wdf.sap.corp/sfsf-platform-core/uxrhp-cardsvc.git'
    SEMANTIC_VERSION = sh(script:'''cat gradle.properties | egrep "componentVersion=" | sed "s/componentVersion=\\(.*\\)/\\1/"''', returnStdout:true).trim()
    RELEASE_VERSION = "${SEMANTIC_VERSION}-${(new Date()).format('yyyyMMddHHmmss', TimeZone.getTimeZone('UTC'))}-${GIT_COMMIT.take(7)}-${env.BUILD_NUMBER}"

  }
  parameters {
    booleanParam(name: 'SKIP_SECURITY', defaultValue: false, description: 'Skip Security Gates')
  }
  agent any
  stages {
    stage('Stage 1') {
      environment {
        BASE_TAG = "${RELEASE_VERSION}"
      }
      steps {
        echo "Semantic version = ${SEMANTIC_VERSION}"
        echo "Release version = ${RELEASE_VERSION}"
        echo 'Hello world!'
        dir('k8s-configuration') {

          sh 'ls -l'
          echo "Base Tag: ${BASE_TAG}"
        }
      }
    }
    stage('Generate Service Image Version') {
      steps {
        script {
             def semanticVersion =  sh(script:'''cat gradle.properties | egrep "componentVersion=" | sed "s/componentVersion=\\(.*\\)/\\1/"''', returnStdout:true).trim()
             def shortCommitID = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
             def timestamp = new Date().format('yyyyMMddHHmmss', TimeZone.getTimeZone('UTC'))
             def serviceImageVersion = "${semanticVersion}-${timestamp}-${shortCommitID}-${env.BUILD_NUMBER}"
             echo "The Service Image Tag is ${serviceImageVersion}"
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
        echo 'Stage 2'
        script {
          def fullCommitId = sh(returnStdout: true, script: 'git log -1 --pretty=format:\'%H\'')?.trim()
          def gitBranchUrlWithCommitId = "${SOURCE_CODE_REPO}@${BRANCH_NAME}:${GIT_COMMIT}"
          println gitBranchUrlWithCommitId
          def gitUrl = scm.getUserRemoteConfigs()[0].getUrl() + "@${env.BRANCH_NAME}:${fullCommitId}"
          println gitUrl
        }
      }
    }
  }
}
#!groovy
pipeline {
  agent any

  environment {
    GIT_COMMIT_SHORT = sh(  script: "printf \$(git rev-parse --short ${GIT_COMMIT})", returnStdout: true)
  }

  stages {

    stage('Build') {
      steps {
        sh script: '''
        git checkout main
        echo $GIT_COMMIT
        pwd
        echo "I am in the main folder"
        docker build --tag cloudgenius/pyflask:$GIT_COMMIT .
        ''', label: "Building docker image for pyflash"
      }
    }
    
    
    
    stage('Push to container registry') {
      steps {
        sh script: '''
        echo $D_PASS | docker login --username $D_USER --password-stdin
        docker push  cloudgenius/pyflask:$GIT_COMMIT
        ''', label: "Pushing images container registry"
      }
    }
    
    
    
   

  }

  post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}

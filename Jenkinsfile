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

    
    
    
    stage('Production deploy') {
      steps {
        sh script: '''
          # grant k8s permissions
          echo $GKE_SERVICE_ACCOUNT | base64 --decode > /tmp/gke_key.json
          gcloud auth activate-service-account --key-file /tmp/gke_key.json
          gcloud config set project $GKE_PROJECT_ID
          gcloud config set compute/zone $GKE_COMPUTE_ZONE
          gcloud --quiet container clusters get-credentials $GKE_CLUSTER_NAME
          # edit k8s deployment
          kubectl get deploy
          kubectl set image deployment/pyflask-deploy pyflask-deploy=cloudgenius/pyflask:$GIT_COMMIT
          kubectl annotate deployment.v1.apps/pyflask-deploy kubernetes.io/change-cause=$GIT_COMMIT
          kubectl get deploy
        ''', label: "Deploying the python-app to production kubernetes cluster"
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

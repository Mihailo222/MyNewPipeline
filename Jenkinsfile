
pipeline {

agent {
  label 'agent1'
}
environment {
  DOCKERHUB_SVC=credentials('dockerhub-svc-account')
}
stages {

  stage('Whoami'){

    steps{
     echo "Stage: ${env.STAGE_NAME}"
      echo "DOCKERHUB_SVC: $DOCKERHUB_SVC"
      echo "DOCKERHUB_SVC_PWD: $DOCKERHUB_SVC_PWD"
    }
  }

  
}

}

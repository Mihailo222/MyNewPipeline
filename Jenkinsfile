
pipeline {

agent {
  label 'agent1'
}
environment {
  DOCKERHUB_SVC=credentials('dockerhub-svc-account')
}
stages {

  stage('Whoami'){
     echo "Stage: ${env.STAGE_NAME}"
  
  }

  
}

}

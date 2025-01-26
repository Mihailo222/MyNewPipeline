
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
      // after injecting a username, password type secret into an environment of a pipeline, you can use it's fileds as
      // ENV_NAME_USR or ENV_NAME_PSW
      echo "DOCKERHUB_SVC: $DOCKERHUB_SVC_USR"
      echo "DOCKERHUB_SVC_PWD: $DOCKERHUB_SVC_PSW"
    }
  }

  stage('Login to DockerHub'){
    steps {
  
    sh('docker login --username $DOCKERHUB_SVC_USR --password $DOCKERHUB_SVC_PSW')
    }
    }

  
}

}

String[] service_Accounts=["failing_sa","dockerhub-svc-account"]

pipeline {

  agent {
  label 'agent1'
}
environment {
  SERVICE_ACCOUNTS = "failing_sa,dockerhub-svc-account"
  //DOCKERHUB_SVC_USR=credentials('dockerhub-svc-account')
}
stages {

  /*stage('Whoami'){

    steps{
     echo "Stage: ${env.STAGE_NAME}"
      // after injecting a username, password type secret into an environment of a pipeline, you can use it's fileds as
      // ENV_NAME_USR or ENV_NAME_PSW
      echo "DOCKERHUB_SVC: $DOCKERHUB_SVC_USR"
      echo "DOCKERHUB_SVC_PWD: $DOCKERHUB_SVC_PSW"
    }
  }*/

  /*stage('Login to DockerHub'){
    steps {
  
    sh('docker login --username $DOCKERHUB_SVC_USR --password $DOCKERHUB_SVC_PSW')
    sh('rm  -rf /root/.docker')
    }
    }*/

  // sad ces posle ovoga da probas da ucitas ovo kao niz kredencijala tj. objekata tipa credential
  stage('Set service account env vars'){
     
    steps {
    script {
  //    def credentials = []
      def serviceAccounts = env.SERVICE_ACCOUNTS.split(",")
      def counter = 1
      serviceAccounts.each { svc_acc -> 
        env."creds_${counter}" = credentials(svc_acc)
        counter++
      }
    }
  }
  }

  stage("See service accounts injected as pipeline environment variables"){
        steps {
          
        /*  echo "SA_1 USER: $creds_1_USR"
          echo "SA_1_PASSWD: $creds_1_PSW"

          echo "SA_2_USER: $creds_2_USR"
          echo "SA_2_PASSWD: $creds_2_PSW"*/

          echo "SA_1 : $creds_1"
          echo "SA_2 : $creds_2"
        }
  }
  
 }
  

  
}













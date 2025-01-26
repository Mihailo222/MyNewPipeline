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
  
stage('Set service account env vars'){
    steps {
    script {
      def serviceAccounts=returnServiceAccounts(env.SERVICE_ACCOUNTS)
      findValidServiceAccount(serviceAccounts)
      // ODRADI DOCKER LOGIN PREKO KREDENCIJALA KOJI SU INJECTOVANI U NAS ENVIRONMENT
    }
  }
}

  /*stage("See service accounts injected as pipeline environment variables"){
        steps {
          
          echo "SA_1 USER: $creds_1_USR"
          echo "SA_1_PASSWD: $creds_1_PSW"

          echo "SA_2_USER: $creds_2_USR"
          echo "SA_2_PASSWD: $creds_2_PSW"
          echo "SA_1_USR : $creds_1_USR"
          echo "SA_2_PSW : $creds_2_PSW"
        }
  }*/
  }  
}

// accept service accounts and trnslate them into an array of maps
def returnServiceAccounts(String serviceAccountsString){
      def credentials = [] 
      def serviceAccounts = serviceAccountsString.split(',')
        

      def counter = 1
  
      serviceAccounts.each { svc_acc ->
      withCredentials([ 
        usernamePassword(credentialsId: "${svc_acc}", usernameVariable: 'SVCUSERNAME', passwordVariable: 'SVCPASSWD')
      ]){
        
    //    env."creds_${counter}_USR"="${SVCUSERNAME}"
    //    env."creds_${counter}_PSW" ="${SVCPASSWD}"
        
        def cred = [
          "credentialsId":"${svc_acc}",
          "username":"${SVCUSERNAME}",
          "password":"${SVCPASSWD}"
        ]
        
        credentials.add(cred)
        }
        counter++
        }
  
return credentials 
}

//check if service account is valid
def checkServiceAccount(String username, String password){

  String cmd="docker login --username \"${username}\" --password \"${password}\""
 def status=sh(script: """
                  '#!/bin/sh -e\n' + $cmd
                  """,  returnStatus: true
                  )
    sh(script: """
    rm -rf /root/.docker
    """, returnStatus: false
    )
    return status
}


def findValidServiceAccount(def serviceAccounts) {
  for ( def svc_acc : serviceAccounts ){
    int status = checkServiceAccount(svc_acc.username, svc_acc.password)

    if(status == 0){
      echo "SUCCESSFULLY LOGGED IN WITH SERVICE ACCOUNT $svc_acc.credentialsId ."
      env.SVC_CREDS_USR="$svc_acc.username"
      env.SVC_CREDS_PSW="$svc_acc.password"
      return
    }
    else {
      echo "FAILED LOGGING IN WITH SERVICE ACCOUNT $svc_acc.credentialsId ."
    }
  }
}

















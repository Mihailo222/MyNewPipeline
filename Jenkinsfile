pipeline {

  agent {
  label 'agent1'
}
environment {
  SERVICE_ACCOUNTS = "failing_sa,dockerhub-svc-account"
}
stages {

stage('Set service account env vars'){
    steps {
    script {
      def serviceAccounts=returnServiceAccounts(env.SERVICE_ACCOUNTS)
      findValidServiceAccount(serviceAccounts)
    
    }
  }
}

  }  
}

// accept service accounts and translate them into an array of objects
def returnServiceAccounts(String serviceAccountsString){
      def credentials = [] 
      def serviceAccounts = serviceAccountsString.split(',')
        

     
  
      serviceAccounts.each { svc_acc ->
      withCredentials([ 
        usernamePassword(credentialsId: "${svc_acc}", usernameVariable: 'SVCUSERNAME', passwordVariable: 'SVCPASSWD')
      ]){
        
        def cred = [
          "credentialsId":"${svc_acc}",
          "username":"${SVCUSERNAME}",
          "password":"${SVCPASSWD}"
        ]
        
        credentials.add(cred)
        }
        
      }
  
return credentials 
}

//check if service account is valid
def checkServiceAccount(String username, String password){

  String cmd="docker login --username \"${username}\" --password \"${password}\""
 def status=sh(script: """
                  set +x                   
                  $cmd
                  set -x
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

















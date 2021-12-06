@Library("buildResults") _
pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: maven
            image: maven:alpine
            command:
            - cat
            tty: true
        '''
    }
  }
  
  triggers {
        eventTrigger jmespathQuery("repository.full_name=='dylanmeh/Using_Webhook2'")
  }
  
  stages {
    stage('Maven build packaging and testing') {
      steps {
        container('maven') {
          sh 'mvn clean package'
        }
      }
    }
  }  
  post {
    success {
      buildResults("Successful")
    }
    
    failure {
      buildResults("Failure")
    }
  }
}    

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
      buildResults("Successfull")
    }
    
    failure {
      builtResults("Failure")
    }
  }
}    

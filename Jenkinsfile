@Library("buildResultsSuccess") _
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
      buildResults()
    }
    
    failure {
      emailext (
      subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """FAILURE: Job '${JOB_NAME} [${BUILD_NUMBER}]':
      Check console output at ${BUILD_URL}""",
      to: 'dylan.mehmedovic@concanon.com'
      )
    }
  }
}    

properties([pipelineTriggers([githubPush()])])
node('linux') {
  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'cc1cf810-2c68-4242-be5b-6b202719de49', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    stage('Test') {
      sh 'ant'
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
  }
}

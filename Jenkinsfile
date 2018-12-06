properties([pipelineTriggers([githubPush()])])
node('linux') {
  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'cc1cf810-2c68-4242-be5b-6b202719de49', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    stage('Test') {
      sh 'ant'
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
    stage('Build') {
      sh 'ant'
    }
    stage('Deploy') {
      sh 'aws s3 cp ${WORKSPACE}/*/rectangle-${BUILD_NUMBER}.jar s3://missdoh-s3-bucket/rectangle-${BUILD_NUMBER}.jar'
    }
    stage('Report') {
      sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
    }
  }
}

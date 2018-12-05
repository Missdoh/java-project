properties([pipelineTriggers([githubPush()])])
node('linux') {
  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '3e7d0d38-cbc8-4a09-bd89-ca0cd0818120', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    stage('Test') {
      sh 'ant'
      sh 'ant -f test.xml -v'
      junit 'reports/result.xml'
    }
    stage('Build') {
      sh 'ant'
      sh 'ant -f build.xml -v'
    }
    stage('Deploy') {
      sh 'aws s3 cp ${WORKSPACE}/*/rectangle-${BUILD_NUMBER}.jar s3://missdoh-s3-bucket/rectangle-${BUILD_NUMBER}.jar'
    }
    stage('Report') {
      sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
    }
  }
}

properties([pipelineTriggers([githubPush()])])

node('linux') {
    stage('Unit Tests') {
        git 'https://github.com/bmtay/java-project.git'
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
    }
    stage('Build') {
        sh 'ant -f build.xml -v'
    }
    stage('Deploy') {
        sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://taylor-seis665demo/'
    }
    stage('Report') {
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkinsID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
        {
            // some block
            sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1'
        }
    }      
}

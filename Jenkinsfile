properties([pipelineTriggers([githubPush()])])

node('linux') {
    stage('Test') {
        git 'https://github.com/bmtay/java-project.git'
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
    }
    stage('Build') {
        sh 'ant -f build.xml -v'
    }
    stage('Deploy') {
        sh 'aws s3 cp rectangle-${env.BUILD_NUMBER}.jar s3://taylor-seis665demo'
    }
    stage('Report) {
        sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1'
    }
}

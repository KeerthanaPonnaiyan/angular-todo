def VERSION = "${env.BUILD_NUMBER}"
def DIST_ARCHIVE = "dist.${env.BUILD_NUMBER}"
def S3_BUCKET = 'source-bucket-demo14'

node {
   agent any
    // Define the label of the agent where the build will run
    // You might want to specify a label that matches the required agent
    
    // Define the environment for the pipeline
    // def nodeHome = tool name: 'Angular Project', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    // env.PATH = "${nodeHome}/bin:${env.PATH}"

        // stage('checkout') {
        //     git branch: 'main'  url: 'https://github.com/KeerthanaPonnaiyan/angular-todo.git'
        //   }

    // try {
        stage('NPM Install') {
            script {
                notifyBitbucket(buildStatus: 'INPROGRESS')
            }
            sh 'npm install --verbose -d'
        }
        stage('Test') {
            sh 'npm run server'
        }
        stage('Run') {
            sh 'npm start'
        }
        stage('Build') {
            sh 'ng build'
        }
        stage('Unit Test') {
            sh 'ng test'
        }
        stage('Archive') {
            sh "cd dist && zip -r ../${DIST_ARCHIVE}.zip . && cd .."
            archiveArtifacts artifacts: "${DIST_ARCHIVE}.zip", fingerprint: true
        }
        // stage('Deploy') {
        //     sh "aws s3 cp ${DIST_ARCHIVE}.zip s3://${S3_BUCKET}/${DIST_ARCHIVE}.zip --profile=default"
        //     sh "aws deploy create-deployment --application-name Angular --deployment-config-name CodeDeployDefault.AllAtOnce --deployment-group-name angular-deployment-group --s3-location bucket=${S3_BUCKET},bundleType=zip,key=${DIST_ARCHIVE}.zip"
        // }
      stage('deploy') {
              sh "aws configure set region $AWS_DEFAULT_REGION" 
              sh "aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID"  
              sh "aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY"
              sh "aws s3 cp angular-todo/src/index.html s3://source-bucket-demo14"
        }
      }
    
// }

pipeline {
    agent any

    environment {
        DOCKER_ID = credentials('DOCKER_ID')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        AWS_DEFAULT_REGION="us-east-1"
        THE_BUTLER_SAYS_SO=credentials('cmb-aws-cred')
    }

    stages {
        stage('Init') {
            steps {
                echo 'Initializing..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "Current branch: ${env.BRANCH_NAME}"
                sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_ID --password-stdin'
            }
        }
        stage('AWS') {           
            steps {
                echo 'AWS command..'
                sh '''
                  aws --version
                  aws ec2 describe-instances
                '''
            }
        }
//        stage ('push artifact') {
//            steps {
//                    sh 'mkdir archive15'
//                    sh 'echo test > archive15/test15.txt'
//                    sh 'zip -r test15.zip archive15'
//                //  zip zipFile: 'test9.zip', archive: false, dir: 'archive9'
//                    sh 'ls'
//                    sh 'cd $WORKSPACE/archive15 && ls'
//                    sh 'aws s3 ls'
//                    sh 'aws s3 cp $WORKSPACE/test15.zip s3://create-lambda-from-zip-file/'
//                //  sh 'aws s3 cp $WORKSPACE/archive5 s3://create-lambda-from-zip-file/ --recursive --include "*"'                
//                    archiveArtifacts artifacts: 'test15.zip', fingerprint: true
//            }
//        }      
//        stage('new pull artifact new') {
//            steps {                
//                    copyArtifacts filter: 'test13.zip', fingerprintArtifacts: true, projectName: env.JOB_NAME, selector: specific(env.BUILD_NUMBER)
//                //  unzip zipFile: 'test11.zip', dir: './archive_new'
//                    sh 'unzip test13.zip -d ./archive_new'
//                    sh 'cat archive_new/test13.txt'                
//            }
//        }        
        stage('Deploy') {
            steps {
                echo 'Removing unused docker containers and images..'
                sh "aws cloudformation create-stack --stack-name s3bucket --template-body file://simplests3cft.json --region 'us-east-1'"
            }
        }
    }
}

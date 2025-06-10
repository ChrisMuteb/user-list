pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'eu-west-1'
        S3_BUCKET = 'user-list'
    }
    parameters {
        booleanParam (
            defaultValue: false,
            description: 'Sync with s3 bucket',
            name: 'MY_SYNC_PARAM'
        )
    }
    stages {
        stage('Build'){
            steps {
                echo 'hello user list'
            }
        }
        stage('Test'){
            steps {
                echo 'Test user list'
            }
        }
        stage('Deploy'){
            when {
                expression {
                    params.MY_SYNC_PARAM
                }
            }
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id-lsp', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key-lsp', variable: 'AWS_SECRET_ACCESS_KEY')
                ]){
                    echo 'Deploy: user list...'
                    sh '''
                        echo "Deploying to s3..."
                        aws s3 sync . s3://$S3_BUCKET \
                        --delete \
                        --exclude ".git/*" \
                        --exclude "Jenkinsfile" \
                        --exclude "*.md"
                     '''
                }
            }
        }
    }


}
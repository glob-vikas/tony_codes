pipeline {
    agent any
    environment {
        LAMBDA_TGT_ZIP = "src.zip"
    }
    stages {
        stage("Zip Lambda Package") {
            steps {
                sh 'cd ./dist && zip -r ${LAMBDA_TGT_ZIP} .'
                sh(
                '''
                if [ $(du -ms ./dist/${LAMBDA_TGT_ZIP} | cut -f1) -gt 50 ]; then \
                    echo $(du -ms ./dist/${LAMBDA_TGT_ZIP} | cut -f1) exceeds 50MB; \
                    false; \
                fi
                '''
                )
            }
        }
        stage("Deploy Lambda to AWS Cloud using Terraform ???") {
            steps {
                timeout(time: 60, unit: 'SECONDS') {
                    input 'Deploy to AWS Cloud?'
                }
                sh 'echo $PWD'
                sh 'cd ./aws_data && terraform init -input=false'
                sh 'echo $PWD'
                sh 'cd ./aws_data && terraform plan -out aws_data.plan -input=false'
                sh 'cd ./aws_data && terraform apply aws_data.plan'
            }
        }
    }
}

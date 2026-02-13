pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        TF_IN_AUTOMATION = "true"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Commands') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-s3-deploy-creds'
                ]]) {
                    sh '''
                        export AWS_DEFAULT_REGION=$AWS_REGION
                        terraform init
                        terraform validate
                        terraform plan -out=tfplan
                    '''
                }
            }
        }

        stage('Terraform Apply (Only PROD)') {
            when {
                branch 'prod'
            }
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-s3-deploy-creds'
                ]]) {
                    sh '''
                        export AWS_DEFAULT_REGION=$AWS_REGION
                        terraform apply -auto-approve tfplan
                    '''
                }
            }
        }
    }
}
            }
        }

        stage('Terraform Apply (Only PROD)') {
            when {
                branch 'prod'
            }
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-s3-deploy-creds'
                ]]) {
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }
    }
}

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

        stage('Terraform Init') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-s3-deploy-creds'
                ]]) {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
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

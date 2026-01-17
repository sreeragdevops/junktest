pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
    }

    parameters {
        booleanParam(
            name: 'DESTROY',
            defaultValue: false,
            description: 'Set TRUE to destroy Terraform infrastructure'
        )
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sreeragdevops/junktest.git'
            }
        }

        stage('Terraform Init') {
            steps {
                withCredentials([
                    aws(
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-jenkins-demo'
                    )
                ]) {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan / Apply') {
            when {
                expression { !params.DESTROY }
            }
            steps {
                withCredentials([
                    aws(
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-jenkins-demo'
                    )
                ]) {
                    sh 'terraform apply -auto-approve'
                }
            }
        }

        stage('Terraform Destroy') {
            when {
                expression { params.DESTROY }
            }
            steps {
                withCredentials([
                    aws(
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-jenkins-demo'
                    )
                ]) {
                    sh 'terraform destroy -auto-approve'
                }
            }
        }
    }

    post {
        success {
            echo "Terraform operation completed successfully 🎉"
        }
        failure {
            echo "Terraform operation failed ❌"
        }
    }
}

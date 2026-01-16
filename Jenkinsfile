pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-1"   // change if needed
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sreeragdevops/jenkins.git'
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

        stage('Terraform Plan') {
            steps {
                withCredentials([
                    aws(
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-jenkins-demo'
                    )
                ]) {
                    sh 'terraform plan'
                }
            }
        }

        stage('Terraform Apply') {
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
    }

    post {
        success {
            echo "Terraform execution completed successfully 🎉"
        }
        failure {
            echo "Terraform execution failed ❌"
        }
    }
}

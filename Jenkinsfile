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

        stage('Terraform Operations') {
            steps {
                withCredentials([
                    aws(
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'aws-jenkins-demo'
                    )
                ]) {

                    sh 'terraform init'

                    script {
                        if (params.DESTROY) {
                            sh 'terraform destroy -auto-approve'
                        } else {
                            sh 'terraform apply -auto-approve'
                        }
                    }
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

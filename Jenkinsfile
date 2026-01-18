pipeline {
    agent any

    parameters {
        choice(
            name: 'TERRAFORM_ACTION',
            choices: ['plan', 'apply', 'destroy'],
            description: 'Select Terraform action to perform'
        )
        string(
            name: 'USER_NAME',
            defaultValue: 'Sreerag',
            description: 'Who triggered the build'
        )
    }

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sreeragdevops/junktest.git'
            }
        }

        stage('Terraform Execution') {
            steps {
               withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
    // some block
}

                    sh 'terraform init'

                    script {
                        if (params.TERRAFORM_ACTION == 'plan') {
                            sh 'terraform plan'
                        }
                        else if (params.TERRAFORM_ACTION == 'apply') {
                            sh 'terraform apply -auto-approve'
                        }
                        else if (params.TERRAFORM_ACTION == 'destroy') {
                            sh 'terraform destroy -auto-approve'
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Terraform ${params.TERRAFORM_ACTION} completed by ${params.USER_NAME} 🎉"
        }
        failure {
            echo "Terraform ${params.TERRAFORM_ACTION} failed ❌"
        }
    }
}


pipeline {
    agent any
    parameters {
        choice(name: 'TERRAFORM_ACTION', choices: ['apply', 'destroy', 'plan'], description: 'Select Terraform action to perform')
        string(name: 'USER_NAME', defaultValue: 'Sreerag', description: 'Specify who is running the code')
    }


    
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-jenkins-demo')
    }
    stages {
        stage('Code checkout from Git') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'aws-jenkins-demo',
                        url: 'https://github.com/sreeragdevops/junktest.git'
                    ]]
                ])
            }
        }

        stage('terraform init') {
            steps {
                script {
                    sh "terraform init"
                }
            }
        }
        stage('terraform apply') {
            steps {
                script {
                    sh "terraform apply --auto-approve"
                }
            }
        }
        stage('terraform destroy') {
            steps {
                script {
                    sh "terraform destroy --auto-approve"
                }
            }
        }
    }
}

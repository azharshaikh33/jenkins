pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select the environment') 
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Select if you want to apply or destroy')
        }

    options {
        ansiColor('xterm')
    }
    stages {
        stage('Terraform Create Network') {
            steps {
                git branch: 'main' url: 'https://github.com/azharshaikh33/terraform-vpc.git'
                sh "terrafile -f env-dev/Terrafile"
                sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars"
                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                sh "terraform ${ACTION} -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
            }
        }

        stage('Terraform Create Databases') {
            steps {
                git branch: 'main', url: 'https://github.com/azharshaikh33/terraform-databases.git'
                sh "terrafile -f env-dev/Terrafile"
                sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars"
                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                sh "terraform ${ACTION} -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
            }
        }
        
    }
}




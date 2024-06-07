pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select the environment') 
        }

    options {
        ansiColor('xterm')
    }
    stages {
        
        stage('Terraform Destry Databases') {
            steps {
                git branch: 'main', url: 'https://github.com/azharshaikh33/terraform-databases.git'
                sh "terrafile -f env-dev/Terrafile"
                sh "terraform init -reconfigure -backend-config=env-${ENV}/${ENV}-backend.tfvars"
                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
            }
        }

        stage('Terraform Destroy Network') {
            steps {
                git branch: 'main', url: 'https://github.com/azharshaikh33/terraform-vpc.git'
                sh "terrafile -f env-dev/Terrafile"
                sh "terraform init -reconfigure -backend-config=env-${ENV}/${ENV}-backend.tfvars"
                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
            }
        }        
    }
}




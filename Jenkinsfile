pipeline {
  agent any

  stages {
    stage('infra only') {
        when { changeset "infra/**"}
        steps {
            echo "infra folder has been changed"
        }
    }

    stage('Terraform Init & Plan'){
        when { anyOf {branch "master";branch "dev";changeRequest()} }
        steps {
            copyArtifacts filter: 'infra/dev/terraform.tfstate', projectName: 'HomeWork1'

            sh '''
            if [ "$BRANCH_NAME" = "master" ] || [ "$CHANGE_TARGET" = "master" ]; then
                cd infra/prod
            else
                cd infra/dev
            fi

            terraform init
            terraform plan
            '''
        }
    }

    stage('Terraform Apply'){
        when { anyOf {branch "master";branch "dev"} }
        input {
            message "Do you want to proceed for infrastructure provisioning?"
        }
        steps {
            copyArtifacts filter: 'infra/dev/terraform.tfstate', projectName: 'HomeWork1'
            sh '''
            if [ "$BRANCH_NAME" = "master" ] || [ "$CHANGE_TARGET" = "master" ]; then
                INFRA_ENV=infra/prod
            else
                INFRA_ENV=infra/dev
            fi
            cd infra/dev
            terraform apply -auto-approve
            '''
            archiveArtifacts artifacts: 'infra/dev/terraform.tfstate', onlyIfSuccessful: true
        }
    }
  }
}



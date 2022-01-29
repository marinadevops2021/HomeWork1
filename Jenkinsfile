pipeline {
  agent any
  options {
        copyArtifactPermission('HomeWork1');
    }
    stages{'Terraform Init & Plan','Terraform Apply'}


  stages {
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
        steps {
            copyArtifacts filter: 'infra/dev/terraform.tfstate', projectName: 'HomeWork1'
            sh '''
            cd infra/dev
            terraform apply -auto-approve
            '''
            archiveArtifacts artifacts: 'infra/dev/terraform.tfstate', onlyIfSuccessful: true
        }
    }
  }
}



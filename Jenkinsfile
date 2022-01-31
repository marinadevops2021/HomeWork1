pipeline {
  agent any

  stages {

//     stage('Terraform Init & Plan'){
//         when { anyOf {branch "master";branch "dev";changeRequest()} }
//         steps {
//             copyArtifacts filter: 'infra/dev/terraform.tfstate', projectName: '${JOB_NAME}'
//
//             sh '''
//             cd infra/dev
//             terraform init
//             terraform plan
//             '''
//         }
//     }

    stage('Terraform Apply'){
        when { anyOf {branch "master";branch "dev"} }
        input {
            message "Do you want to proceed for infrastructure provisioning?"
        }
        steps {
            copyArtifacts filter: 'infra/dev/terraform.tfstate', projectName: '${JOB_NAME}'
            sh '''
            cd infra/dev
            terraform apply -auto-approve
            '''
            archiveArtifacts artifacts: 'infra/dev/terraform.tfstate', onlyIfSuccessful: true
        }
    }
  }
}
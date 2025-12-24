pipeline {
  agent any

  options {
    skipDefaultCheckout(true)
  }

  environment {
    ENV = "${env.BRANCH_NAME}"
    TF_WORKDIR = "environments/${env.BRANCH_NAME}"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: "${env.BRANCH_NAME}",
            url: 'https://github.com/Chandraleka123/infrapipeline.git',
            credentialsId: 'git-credes'
      }
    }

    stage('Terraform Init') {
      steps {
        dir("${TF_WORKDIR}") {
          sh 'terraform init'
        }
      }
    }

    stage('Terraform Validate') {
      steps {
        dir("${TF_WORKDIR}") {
          sh 'terraform validate'
        }
      }
    }

    stage('Terraform Plan') {
      steps {
        dir("${TF_WORKDIR}") {
          sh 'terraform plan -out=tfplan'
          sh 'terraform show -no-color tfplan > tfplan.txt'
          sh 'cat tfplan.txt'
        }
      }
    }

    stage('Approval') {
    
      steps {
        input message: "Approve deployment to production?", ok: 'Deploy'
      }
    }

    stage('Terraform Apply') {
      steps {
        dir("${TF_WORKDIR}") {
          sh 'terraform apply tfplan'
        }
      }
    }
  }
}

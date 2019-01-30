pipeline {
    agent {
        node {
            label 'azure-tools-node01'
        }
      }
    parameters {
        choice(
            name: 'select_env',
            choices: 'prod\ndev',
            description: 'select the environment names'
        )
        choice(
            name: 'ws_option',
            choices: 'select\nnew',
            description: 'workspace option for terraform to run'
        )
        choice(
            name: 'ws_name',
            choices: 'dev_pci',
            description: 'workspace name for terraform to run'
        )
        choice(
            name: 'tfvarsfiles',
            choices: 'devops.tfvars\ncore.tfvars',
            description: 'select terraform parameters file to run'
        )

        choice(
            name: 'deploy',
            choices: 'plan\napply',
            description: 'plan or apply terraform configuration to run'
        )
        choice(
            name: 'approvals',
            choices: '\n-auto-approve',
            description: 'approve terraform'
        )
      }
    environment {
        ENV         = "${params.select_env}"
        WS          = "${params.ws_option}"
        WSNAME      = "${params.ws_name}"
        DEPLOYMENT  = "${params.deploy}"
        TFVARSFILE  = "${params.tfvarsfiles}"
        APPROVE     = "${params.approvals}"
        ARM_SUBSCRIPTION_ID  = credentials("ARM_SUBSCRIPTION_ID")
        ARM_CLIENT_ID       = credentials("ARM_CLIENT_ID")
        ARM_CLIENT_SECRET   = credentials("ARM_CLIENT_SECRET")
        ARM_TENANT_ID       = credentials("ARM_TENANT_ID")
        STORAGE_ACCOUNT_KEY = credentials("STORAGE_ACCOUNT_KEY")
        WORKDIR_CMD         = '/var/lib/jenkins/workspace/vnet_pipeline/'
    }
    stages {
        stage('checkout-repo') {
            steps {
              checkout scm
            }
        }
        stage('init-terraform') {
            steps {
                    sh 'echo $PWD'
		    sh 'cd ${WORKDIR_CMD} && terraform init'
		    sh 'echo $PWD'
            }
        }
        stage('workspace-terraform') {
            steps {
                sh 'cd ${WORKDIR_CMD}'
            }
        }
        stage('deploy-terraform') {
            steps {
                sh 'cd ${WORKDIR_CMD} && terraform $DEPLOYMENT $APPROVE'
            }
        }
    }
}

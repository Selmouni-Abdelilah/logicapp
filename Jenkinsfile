ipeline {
    agent any
    tool name: 'terraform', type: 'terraform'
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHubcredentials', url: 'https://github.com/Selmouni-Abdelilah/logicapp']])
                }
            }
        }
        stage('Azure login'){
            steps{
                withCredentials([azureServicePrincipal('Azure_credentials')]) {
                    sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
                }
            }
        }
        stage('Terraform ') {
            steps {
                script {
                    withCredentials([azureServicePrincipal(credentialsId: 'Azure_credentials',
                                        subscriptionIdVariable: 'SUBS_ID',
                                        clientIdVariable: 'CLIENT_ID',
                                        clientSecretVariable: 'CLIENT_SECRET',
                                        tenantIdVariable: 'TENANT_ID')]) {
                    sh 'terraform init -upgrade'
                    sh "terraform import terraform import azurerm_logic_app_standard.logicapp /subscriptions/${SUBS_ID}/resourceGroups/rg_abdel_proc/providers/Microsoft.Web/sites/logicapp1937"
                }
            }
            }
        }
    }
}
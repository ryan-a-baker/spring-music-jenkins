pipeline {
    agent any
    parameters {
      string(name: 'TANZU_API_TOKEN' , defaultValue: 'Tanzu_Platform_for_K8s_SaaS_-_Internal_1_APItoken') // TPk8s API Token
    }
    stages {
        stage('Build') {
            steps {
                withCredentials([string(credentialsId: 'TANZU_API_TOKEN', variable: 'TANZU_API_TOKEN_API')]) {
                    sh('TANZU_API_TOKEN=$TANZU_API_TOKEN_API tanzu login')
                }
                sh 'tanzu project use pm-demo'
                sh 'ls'
                sh 'tanzu build --output-dir build'
            }
        }
        stage('Deploy-Stage') {
            steps {
                withCredentials([string(credentialsId: 'TANZU_API_TOKEN', variable: 'TANZU_API_TOKEN_API')]) {
                    sh('TANZU_API_TOKEN=$TANZU_API_TOKEN_API tanzu login')
                }
                sh 'tanzu project use pm-demo'
                sh 'tanzu space use spring-music-stage'
                sh 'sed -i "s|http-spring-music-dev|http-spring-music-stage|g" .tanzu/config/k8sGatewayRoutes.yaml'
                sh 'tanzu deploy -y --from-build build'
            }
        }
        stage('Deploy-Prod') {
            steps {
                withCredentials([string(credentialsId: 'TANZU_API_TOKEN', variable: 'TANZU_API_TOKEN_API')]) {
                    sh('TANZU_API_TOKEN=$TANZU_API_TOKEN_API tanzu login')
                }
                sh 'tanzu project use pm-demo'
                sh 'tanzu space use spring-music-prod'
                sh 'sed -i "s|http-spring-music-stage|http-spring-music|g" .tanzu/config/k8sGatewayRoutes.yaml'
                sh 'tanzu deploy -y --from-build build'
                sh 'ls'
            }
        }
    }
}

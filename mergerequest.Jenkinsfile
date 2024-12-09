pipeline {
    agent any

    environment {
        DOCKER_REGISTRY_CREDENTIALS = credentials('docker-hub-credentials')
        SLACK_WEBHOOK = credentials('slack-webhook')
        KUBECONFIG_FILE = credentials('kubeconfig-file')
        GITHUB_CREDENTIALS = credentials('github-credentials') 
        DOCKERIMAGENAME = "anitha/nodejsproject"
        dockerImage = ""
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/anithagurjarathi/nodejsproject.git',
                        credentialsId: 'github-credentials'
                    ]]
                ])
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
        
        stage('SonarQube Analysis') {
            environment {
                SONARQUBE_AUTH = credentials('sonarqube-token')
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=nodejsproject -Dsonar.sources=. -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONARQUBE_AUTH_USR'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build DOCKERIMAGENAME
                }
            }
        }

        stage('Pushing Image') {
            steps{
                script {
                docker.withRegistry( 'https://registry.hub.docker.com', DOCKER_CREDENTIALS ) {
                    dockerImage.push("latest")
                }
                }
            }
        }

        stage('Deploy deployment and service file') {
            steps {
                script {
                    kubernetesDeploy(configs: "deployment.yaml", "service.yaml",
                    kubeconfigID: 'my-kubeconfig')
            }
        }
        }
    }

    post {
        success {
            slackSend(channel: 'deployment_notifications', color: 'good', message: 'Deployment of Node.js app has been succesfull')
        }
        failure {
            slackSend(channel: 'deployment_notifications', color: 'danger', message: 'Deployment of Node.js app failed, Pls take a look')
        }
    }
}
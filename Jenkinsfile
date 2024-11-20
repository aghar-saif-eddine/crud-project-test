pipeline {
    agent any

    stages {
        stage('Checkout GitHub repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/aghar-saif-eddine/crud-project-test']])
            }
        }

        stage('Build and Tag Docker Image') {
            steps {
                script {
                    sh 'docker build . -t symfonyapp'
                    sh 'docker tag symfonyapp saif1198/symfonyapp'
                }
            }
        }

        stage('Push the Docker Image to DockerHUb') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub')]) {
                    sh 'docker login -u saif1198 -p ${docker_hub}'
}
                    sh 'docker push saif1198/symfonyapp'
                }
            }
        }

        stage('Deploy deployment and service file') {
            steps {
                script {
                    kubernetesDeploy configs: 'deploymentsvc.yaml', kubeconfigId: 'kubernetes_config'
                }
            }
        }
    }
}
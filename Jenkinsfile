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
        stage('Push the Docker Image to DockerHub') {
            steps {
                script {
                    // Use Jenkins credentials for secure Docker login
                    withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub_password')]) {
                    // Log into Docker Hub
                    sh 'echo "$docker_hub_password" | docker login -u saif1198 --password-stdin'
                }
                    // Push the image to Docker Hub
                    sh 'docker push saif1198/symfonyapp'
                        }
                    }
                }
        

        stage('Deploy deployment and service file') {
            steps {

                 // Use the stored kubeconfig from Jenkins credentials
                withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG')]) {
                    script {
                        sh "whoami"
                        sh "echo 'Using KUBECONFIG: $KUBECONFIG'"
                        sh "cat $KUBECONFIG"

                        // Verify access to the Kubernetes cluster
                        sh "kubectl get nodes --v=9" // Verbose output for debugging

                        // Deploy the Kubernetes configuration using the kubeconfig
                        kubernetesDeploy(
                            configs: 'deploymentsvc.yaml',  // Path to your Kubernetes YAML file
                            kubeconfigId: 'minikube-kubeconfig'  // This should match your credentialsId
                        )
                    }
                }
            }
        }
        }
}

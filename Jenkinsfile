pipeline {

    environment {
        dockerimagename = "jagkush/react-app"
        dockerImage = ""
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main ', url: 'https://github.com/jagkush/jenkins_ci_cd.git'
            }
        }


        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }


        stage('Pushing image') {
            environment {
                registryCredential = 'dockerhub-cred'
            }
            steps{
                script {
                    docker.withRegistry(
                        'https://registry.hub.docker.com', 
                        registryCredential 
                    ) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying React.js container to Kubernetes') {
            steps{
                script {
                    KubernetesDeploy(
                        configs: "deployment.yaml", "service.yaml"
                    )
                }
            }
        }
    }
}
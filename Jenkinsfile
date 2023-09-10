pipeline {
    environment {
        dockerimaagename = "jagkush/react-app"
        dockerimage = ""
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/jagkush/jenkins_ci_cd.git'
            }
        }


        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build dockerimaagename
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
                    ){
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying React.js container to Kubernetes') {
            steps{
                script {
                    KubernetesDeploy(
                        "deployment.yaml", "service.yaml"
                    )
                }
            }
        }
    }
}
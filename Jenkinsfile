pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }

    environment {
        imageTag = "lutfar1996/swiggy-app" // Base image name without tag
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Lutfar1996/Swiggy-Application.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }

        stage("Docker Build & Push") {
            steps {
                script {
                    def imageTagWithBuildId = "${imageTag}:${env.BUILD_ID}" // Construct full image tag
                    echo "Building and pushing image: ${imageTagWithBuildId}" // Debug print
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t ${imageTagWithBuildId} ."
                        sh "docker push ${imageTagWithBuildId}"
                    }
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    def imageTagWithBuildId = "${imageTag}:${env.BUILD_ID}" // Use the full image tag
                    echo "Deploying image: ${imageTagWithBuildId}" // Debug print
                    withAWS(credentials: 'aws', region: 'us-west-1') {
                        sh 'aws eks --region us-west-1 update-kubeconfig --name EKS_CLOUD'
                        
                        // Replace PLACEHOLDER in deployment-service.yml with the actual image tag
                        sh "sed 's|PLACEHOLDER|${imageTagWithBuildId}|g' deployment-service.yml | kubectl apply -f -"
                        
                        // Set the image for the deployment
                        sh "kubectl set image deployment/swiggy swiggy=${imageTagWithBuildId}"
                        
                        sh "kubectl rollout status deployment/swiggy"
                    }
                }
            }
        }

        stage('Get Load Balancer DNS') {
            steps {
                withAWS(credentials: 'aws', region: 'us-west-1') {
                sh 'kubectl get svc swiggy-service -o jsonpath="{.status.loadBalancer.ingress[0].hostname}"'
        }
    }
}
    }
}

pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    // environment {
    //     SCANNER_HOME=tool 'sonar-scanner'
    // }
    //
       environment {
          imageTag ="lutfar1996/swiggy-app" 
       }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/Lutfar1996/Swiggy-Application.git'
            }
        }
        // stage("Sonarqube Analysis "){
        //     steps{
        //         withSonarQubeEnv('sonar-server') {
        //             sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Amazon \
        //             -Dsonar.projectKey=Amazon '''
        //         }
        //     }
        // }
        //  stage("quality gate"){
        //    steps {
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'jenkins' 
        //         }
        //     } 
        // }
        stage('Install Dependencies') {
            steps {
                // sh "npm update"
                sh "npm install"
            }
        }
        // stage('OWASP FS SCAN') {
        //     steps {
        //         dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
        //         dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        //     }
        // }
        // stage('TRIVY FS SCAN') {
        //     steps {
        //         sh "trivy fs . > trivyfs.txt"
        //     }
        // }
        stage("Docker Build & Push"){
            steps{
                script {
                    def imageTagWithBuildId = "${imageTag}:${env.BUILD_ID}" // Construct full image tag
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t ${imageTagWithBuildId} ."
                        sh "docker push ${imageTagWithBuildId}"
                    }
                }
            }
        }
        // stage("TRIVY"){
        //     steps{
        //         sh "trivy image lutfar1996/amazon-clone:latest > trivyimage.txt" 
        //     }
        // }
        // stage('Deploy to container'){
        //     steps{
        //         sh 'docker run -d --name amazon-clone -p 3000:3000 lutfar1996/amazon-clone:latest'
        //     }
        // }

        stage('Deploy to EKS') {
            steps {
                 script {
                    def imageTagWithBuildId = "${imageTag}:${env.BUILD_ID}" // Use the full image tag
                    withAWS(credentials: 'aws', region: 'us-west-1') {
                        sh 'aws eks --region us-west-1 update-kubeconfig --name EKS_CLOUD'
                        sh "kubectl set image deployment/swiggy swiggy=${imageTagWithBuildId}"
                        sh "sed 's|PLACEHOLDER|${imageTagWithBuildId}|g' deployment-service.yml | kubectl apply -f -"
                        sh "kubectl rollout status deployment/swiggy"
                    }
                } 
            }
        }

    }
}
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
          imageTag ="lutfar1996/swiggy-app:${env.BUILD_ID}" 
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
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){  
                       
                       sh "docker build -t ${imageTag} ."
                    //    sh "docker tag amazon-clone lutfar1996/amazon-clone:latest "
                       sh "docker push ${imageTag} "
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
                withAWS(credentials: 'aws', region: 'us-west-1') {
                    sh 'aws eks --region us-west-1 update-kubeconfig --name EKS_CLOUD'
                    sh 'kubectl set image deployment/swiggy swiggy=${imageTag}'
                    sh "sed 's|PLACEHOLDER|${imageTag}|g' deployment-service.yml | kubectl apply -f -"
                    sh "kubectl rollout status deployment/swiggy"
                    
                }
            }
        }

    }
}
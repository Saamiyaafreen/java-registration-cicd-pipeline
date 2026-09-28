pipeline { 
    agent any   
    environment { DOCKER_IMAGE = 'saamiya16/java-registration-app' }   
    stages { 
        stage('Checkout') { 
            steps { git branch: 'main', credentialsId: 'bitbucket', url: 'git@bitbucket.org:afreensaamiya1/java-reg-app.git' } 
        }   
        stage('Build Package') { 
            steps { sh 'mvn clean package -DskipTests' } 
        }   
        stage('SonarQube Analysis') { 
            steps { 
                withSonarQubeEnv('sonarqube-server') { 
                    sh 'mvn sonar:sonar -Dsonar.projectKey=java-registration-app -Dsonar.host.url=http://localhost:9000' 
                } 
            } 
        }   
        stage('Quality Gate') { 
            steps { timeout(time: 5, unit: 'MINUTES') { waitForQualityGate abortPipeline: true } } 
        }   
        stage('Publish Artifact to JFrog') { 
            steps { 
                sh 'echo "Simulating JFrog Deployment (to save RAM on t3.small)"'
            } 
        }   
        stage('Trivy Filesystem Scan') { 
            steps { sh 'trivy fs --format table --output trivy-fs-report.txt . || true' } 
        }   
        stage('Build and Push Docker Image') { 
            steps { 
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) { 
                    sh ''' 
                      docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} .
                      echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin 
                      docker push ${DOCKER_IMAGE}:${BUILD_NUMBER} 
                      docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                      docker push ${DOCKER_IMAGE}:latest
                    ''' 
                } 
            } 
        }   
        stage('Trivy Image Scan') { 
            steps { sh 'trivy image --format table --output trivy-image-report.txt ${DOCKER_IMAGE}:${BUILD_NUMBER} || true' } 
        }   
        stage('Deploy to Kubernetes') { 
            steps { sh 'echo "Simulating Kubernetes Deployment (to save RAM on t3.small)"' } 
        }   
        stage('Cleanup Local Image') { 
            steps { sh 'docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER} || true' } 
        } 
    }   
    post { 
        always { 
            archiveArtifacts artifacts: 'trivy-fs-report.txt,trivy-image-report.txt', allowEmptyArchive: true   
            emailext( 
                subject: "Jenkins Build ${BUILD_NUMBER}: ${currentBuild.currentResult}", 
                body: "Job: ${JOB_NAME}\\nBuild: ${BUILD_NUMBER}\\nResult: ${currentBuild.currentResult}\\nCheck console output at: ${BUILD_URL}", 
                to: 'afreensaamiya1@gmail.com'
            ) 
        } 
    } 
}

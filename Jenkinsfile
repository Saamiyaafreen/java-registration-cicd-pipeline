pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "saamiya16/java-registration-app"
        DOCKER_CREDENTIALS = credentials('dockerhub')
        SONAR_TOKEN = credentials('sonarqube-token')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=java-registration-app -Dsonar.host.url=http://localhost:9000'
                }
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        def customImage = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                        customImage.push()
                        customImage.push('latest')
                    }
                }
            }
        }
        
        // stage('Publish Artifact') {
        //     steps {
        //         // Skipped because Artifactory is not running on t3.small
        //         // sh 'mvn deploy'
        //     }
        // }
    }
}


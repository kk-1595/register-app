pipeline{
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "register-app-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "kk1595"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("cleanup workspace"){
            steps {
               cleanWs()
            }
        }
        stage('Git chekout'){
            steps {
                 git branch: 'main', credentialsId: 'github', url: 'https://github.com/kk-1595/register-app.git'
            }
        }
        stage("maven build"){
            steps {
               sh "mvn clean package"
            }
        }
        stage('Test application'){
            steps {
                sh "mvn test"
            }
        }
        stage("sonarqube analysis"){
            steps {
                script{
                    withSonarQubeEnv(credentialsId: 'Jenkins-Sonarqube-token'){
                    sh "mvn sonar:sonar"    
                    }
                }
            }
        }
         stage("Quality gate"){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-Sonarqube-token'
                }
            }
        }
        stage("docker build & Push"){
            steps {
                script{
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                   }
                }
            }
        }
        stage("trivy scan"){
            steps {
                script {
                    sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image kk1595/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                }
            }
        }
    }
}
        

        


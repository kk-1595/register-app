pipeline{
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
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
    }
}
        


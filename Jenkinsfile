pipeline{
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("cleanup workspace"){
            step {
                cleanWS()
            }
        }
        stage ('Git chekout'){
            step{
                 git branch: 'main', credentialsId: 'github', url: 'https://github.com/kk-1595/register-app.git'

            }
        }
    }
}


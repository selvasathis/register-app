pipeline {
    agent { label 'jenkins_agent'}
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    stages {
        stage ("workspace cleanup") {
            steps {
                cleanWs()
            }
        }
        stage ("checkout from scm") {
            steps {
                git branch: 'main', credentialsId: 'githubpasswd', url: 'https://github.com/selvasathis/register-app.git'
            }
        }
        stage ("build app") {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ("test app") {
            steps {
                sh 'mvn test'
            }
        }
        stage ("sonar validation")  {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-tocken') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage ("docker image build") {
            steps {
                script {
                    sh "docker build -t new ."
                }
            }
        }
    }
}
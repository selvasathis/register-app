pipeline {
    agent { label 'jenkins-agent'}
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    
    environment {
        APP_NAME = "register_app"
        DOCKER_USER = "thendralsathis"
        IMAGE_NAME = "${DOCKER_USER}"+"/"+"${APP_NAME}"
    }
    stages {
        stage ("workspace cleanup") {
            steps {
                cleanWs()
            }
        }
        stage ("checkout from scm") {
            steps {
                git branch: 'main', credentialsId: 'githubpswd', url: 'https://github.com/selvasathis/register-app.git'
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
        // stage ("sonar validation")  {
        //     steps {
        //         script {
        //             withSonarQubeEnv(credentialsId: 'sonar-tocken') {
        //                 sh "mvn sonar:sonar"
        //             }
        //         }
        //     }
        // }
        // stage("quality gate") {
        //     steps {
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'sonar-tocken'
        //         }
        //     }
        // }
        // stage ("docker image build") {
        //     steps {
        //         script {
        //             sh "docker build -t "${IMAGE_NAME}" ."
        //         }
        //     }
        // }
    }
}
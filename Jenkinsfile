pipeline {

    agent any

    stages {
        stage("Build Jar") {
            steps {
                bat "mvn clean package -DskipTests"
            }
        }

        stage("Build Image") {
            steps {
                bat "docker build -t=alexyarm/selenium:latest ."
            }
        }

        stage("Push Image") {
            environment {
                DOCKER_HUB = credentials('dockerhub-cred')
            }

            steps {
                bat 'echo %DOCKER_HUB_PSW% | docker login -u %DOCKER_HUB_USR% --password-stdin'
                bat "docker push alexyarm/selenium:latest"
                bat "docker tag alexyarm/selenium:latest alexyarm/selenium:${env.BUILD_NUMBER}"
                bat "docker push alexyarm/selenium:${env.BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            bat "docker logout"
        }
    }
}
pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'jdk21'
    }

    parameters {
        string(
            name: 'BRANCH_NAME',
            defaultValue: 'main',
            description: 'Enter the Git branch to build'
        )
    }

    environment {
        APP_NAME = "week6demo-app"
        DOCKER_IMAGE = "myrepo/week6demo-app:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}",
                    url: 'https://github.com/AbdulrahmanHamid/JacocoExample'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('JaCoCo Report') {
            steps {
                bat 'mvn jacoco:report'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Build Docker Image') {
            when { branch 'master' }
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }
    }
}

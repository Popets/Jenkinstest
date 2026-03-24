pipeline {
    agent {
        docker {
            image 'eclipse-temurin:21-jdk'
            // image 'maven:3.9.9-eclipse-temurin-21'
            reuseNode true
        }
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven Wrapper') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Run Tests') {
            steps {
                sh './mvnw test'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
        }
        success {
            echo '✅ Build succeeded!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
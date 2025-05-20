pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5'
        jdk 'JDK 11'
    }

    triggers {
        pollSCM('H/2 * * * *') // проверка репозитория каждые 2 минуты
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Test Reports') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Успешная сборка и тестирование.'
        }
        failure {
            echo '❌ Сборка или тесты не прошли.'
        }
    }
}

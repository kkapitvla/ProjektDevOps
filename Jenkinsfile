pipeline {
    agent any
    environment {
        VERSION = "1.0.${env.BUILD_ID}"
    stages {
        stage('Kompilacja kodu') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Testowanie') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Aplikacja') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Docker') {
            steps {
                script {
                    docker.build("twoje-uzytkownik/app:${env.BUILD_ID}")
                }
            }
        }
        stage('Wdrażanie') {
            steps {
                script {
                    try {
                        sh 'docker-compose up -d'
                    } catch (Exception e) {
                        echo 'Wdrażanie nieudane, powrót do poprzednich ustawień...'
                        sh 'docker-compose down'
                        throw e
                    }
                }
            }
        }
    }
    post {
        always {
            mail to: 'k.kapitula.063@studms.ug.edu.pl',
                subject: "Pipeline ${currentBuild.fullDisplayName}",
                body: "Status: ${currentBuild.currentResult}"
        }
        success {
            echo 'Sukces!'
        }
        failure {
            slackSend (color: '#FF0000', message: "Pipeline ${currentBuild.fullDisplayName} failed.")
        }
    }
}

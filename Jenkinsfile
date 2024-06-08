pipeline {
    agent any
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
        stage('Budowanie Docker Image') {
            steps {
                script {
                    docker.build("twoje-uzytkownik/app:${env.BUILD_ID}")
                }
            }
        }
        stage('Wdrożenie') {
            steps {
                script {
                    try {
                        sh 'docker-compose up -d'
                    } catch (Exception e) {
                        echo 'Wdrożenie nie powiodło się, następuje cofnięcie do poprzedniej wersji...'
                        sh 'docker-compose down'
                        throw e
                    }
                }
            }
        }
    }
}

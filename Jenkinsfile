pipeline {
    agent any

    environment {
        IMAGE_NAME = "khadijaib/supmit-static-site"
        TAG = "latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Construction de l’image Docker...'
                bat 'docker build -t %IMAGE_NAME%:%TAG% .'
            }
        }

        stage('Test Image (curl)') {
            steps {
                echo 'Test local de l’image...'
                bat '''
                    docker run -d -p 8087:80 --name supmit-static-container %IMAGE_NAME%:%TAG%
                    timeout /t 5
                    curl -I http://localhost:8087 || exit /b 1
                    docker stop supmit-static-container
                    docker rm supmit-static-container
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_HUB_PASS')]) {
                    bat '''
                        echo %DOCKER_HUB_PASS% > pass.txt
                        docker login -u omellah --password-stdin < pass.txt
                        docker push %IMAGE_NAME%:%TAG%
                        docker logout
                        del pass.txt
                    '''
                }
            }
        }

        stage('Deploy to Review') {
            steps {
                echo 'Déploiement Review (simulé)...'
                bat 'echo "Docker run %IMAGE_NAME%:%TAG% sur une VM AWS (Ubuntu)"'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Déploiement Staging (simulé)...'
                bat 'echo "Déploiement en Staging terminé !"'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Déploiement Production (simulé)...'
                bat 'echo "Déploiement en Production terminé !"'
            }
        }
    }
}
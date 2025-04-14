pipeline {
    agent none

    stages {
        // 1. Checkout aşaması: Kaynak kodları alıyoruz
        stage('Checkout') {
            agent any // Checkout işlemi yapılacak herhangi bir node
            steps {
                checkout scm
            }
        }

        // 2. Docker Build Aşaması
        stage('Build Docker Image') {
            agent any // Docker build için herhangi bir node
            steps {
                script {
                    // Dockerfile bulunduğu dizinde imajı oluşturuyoruz
                    sh 'docker build -t my-web-app .'
                    sh "docker images"
                }
            }
        }

        // 3. Kubernetes Deploy Aşaması
        stage('Deploy Pod to Kubernetes') {
            agent {
                kubernetes {
                    label 'slave' // Kubernetes pod label'ı
                    yamlFile 'pod.yaml'
                }
            }
            steps {
                sh "kubectl get nodes"
                echo 'Deploying pod to Kubernetes...'
                // Pod manifest dosyasına göre pod deploy ediyoruz
                sh 'kubectl apply -f pod.yaml'
                // Pod durumunu kontrol ediyoruz
                sh 'kubectl get pods'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

pipeline {
    agent none

    stages {
        // 1. Checkout aşaması: Kaynak kodları alıyoruz
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // 2. Docker Build Aşaması
        stage('Build Docker Image') {
            agent any
            steps {
                script {
                    // Dockerfile bulunduğu dizinde imajı oluşturuyoruz
                    sh 'docker build -t my-web-app .'
                }
            }
        }

        // 3. Kubernetes Deploy Aşaması
        stage('Deploy Pod to Kubernetes') {
            agent {
                kubernetes {
                    // Kubernetes Cloud'da tanımlı olan pod template'inin label'ı
                    label 'custom-agent'  
                    // Workspace'deki pod.yaml dosyasını kullanarak pod manifestini yükler
                    yamlFile 'pod.yaml'
                }
            }
            steps {
                echo "kubectl get nodes"
                echo 'Deploying pod to Kubernetes...'
                // pod.yaml dosyasındaki tanıma göre pod deploy ediliyor
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

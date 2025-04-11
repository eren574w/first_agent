pipeline {
    agent none

    stages {
        // 1. Checkout aşaması: Kaynak kodları alıyoruz
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }

        // 2. Docker Build Aşaması
        stage('Build Docker Image') {
            agent any // Bu aşamada herhangi bir agent kullanılabilir
            steps {
                script {
                    // Dockerfile bulunduğu dizinde imajı oluşturuyoruz
                    sh 'docker build -t my-web-app .'
                }
            }
        }

        // 3. Kubernetes Deploy Aşaması (Opsiyonel, bu kısmı sonra yapacağız)
        stage('Deploy Pod to Kubernetes') {
            agent kubernetes {
                label 'custom-agent'  // Kubernetes etiketini buraya yazıyoruz
                yamlFile 'pod.yaml'
            }
            steps {
                sh 'kubectl apply -f pod.yaml'
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

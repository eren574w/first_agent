pipeline {
    // Genel olarak tek bir agent tanımlamıyoruz, çünkü aşama bazında farklı agent kullanılacak.
    agent none

    // SCM tetikleme gibi ayarları tutmak için (opsiyonel)
    triggers {
        pollSCM('* * * * *')
    }
    
    stages {
        // 1. Checkout aşaması: Docker işlemleri yapacak agent üzerinde çalıştırılır
        stage('Checkout') {
            agent { 
                label 'docker-agent-alpine' 
            }
            steps {
                // Kaynak kodları repodan alıyoruz
                checkout scm
            }
        }
        
        // 2. Docker İmaj Build Aşaması
        stage('Build Docker Image') {
            agent { 
                label 'docker-agent-alpine' 
            }
            steps {
                echo 'Building Docker image...'
                // Dockerfile'ın bulunduğu dizinde image build ediliyor
                sh 'docker build -t my-web-app .'
            }
        }
        
        // 3. Kubernetes Deploy Aşaması:
        // Bu aşamada, Jenkins Cloud'da tanımlı Kubernetes pod template'ini kullanarak,
        // harici pod.yaml dosyasından agent (custom-agent label) oluşturulacak ve kubectl komutları çalıştırılacak.
        stage('Deploy Pod to Kubernetes') {
            agent {
                kubernetes {
                    // Bu label, Kubernetes Cloud ayarlarınızda tanımlı pod template'inin (custom-agent) label'ı ile uyumlu olmalı.
                    label "custom-agent"
                    // Workspace'deki pod.yaml dosyasını kullanarak pod manifestini yükler.
                    yamlFile 'pod.yaml'
                }
            }
            steps {
                echo 'Deploying pod...'
                // pod.yaml dosyasındaki tanıma göre pod deploy ediliyor.
                sh 'kubectl apply -f pod.yaml'
                // Pod durumunu kontrol ediyoruz.
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

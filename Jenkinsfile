pipeline {
    agent any  // Bu aşamada herhangi bir agent kullanılabilir

    stages {
        // 1. Docker Build Aşaması
        stage('Build Docker Image') {
            steps {
                script {
                    // Dockerfile bulunduğu dizinde imajı oluşturuyoruz
                    sh 'docker build -t my-web-app .'  // Burada my-web-app imajını oluşturuyoruz
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image build completed successfully!'
        }
        failure {
            echo 'Docker image build failed!'
        }
    }
}

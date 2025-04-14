pipeline {
    agent none

    stages {
        // 1. Checkout aşaması: Kaynak kodları çekiyoruz.
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }

        // 2. Docker Build Aşaması: Docker imajı oluşturuluyor.
        stage('Build Docker Image') {
            agent any
            steps {
                script {
                    sh 'docker build -t my-web-app .'
                    sh 'docker images'
                }
            }
        }

        // 3. Kubernetes Deploy Aşaması: Jenkins agent pod üzerinden komutlar çalıştırılıyor.
        stage('Deploy Pod to Kubernetes') {
            agent {
                kubernetes {
                    label 'slave'          // YAML dosyasında "jenkins: slave" etiketi kullanılmış
                    yamlFile 'pod1.yaml'
                }
            }
            steps {
                echo 'Agent pod oluşturuldu. Komutlar container içinde çalışıyor...'
                // Burada artık jnlp container üzerinden Jenkins ile iletişim sağlanıyor;
                // ek pod oluşturma işlemi yapmaya gerek yok.
                sh 'kubectl get nodes'
                sh 'kubectl get pods'
            }
        }
    }

    post {
        success {
            echo 'Pipeline başarıyla tamamlandı!'
        }
        failure {
            echo 'Pipeline hatalı tamamlandı!'
        }
    }
}

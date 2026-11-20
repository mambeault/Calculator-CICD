pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Code source récupéré depuis GitHub'
            }
        }

        stage('Build calc-service') {
            steps {
                dir('calc-service') {
                    sh 'docker build -t 192.168.10.95:5000/calculator-calc:latest .'
                }
            }
        }

        stage('Build sum-service') {
            steps {
                dir('sum-service') {
                    sh 'docker build -t 192.168.10.95:5000/calculator-sum:latest .'
                }
            }
        }

        stage('Build sub-service') {
            steps {
                dir('sub-service') {
                    sh 'docker build -t 192.168.10.95:5000/calculator-sub:latest .'
                }
            }
        }

        stage('Build mul-service') {
            steps {
                dir('mul-service') {
                    sh 'docker build -t 192.168.10.95:5000/calculator-mul:latest .'
                }
            }
        }

        stage('Build div-service') {
            steps {
                dir('div-service') {
                    sh 'docker build -t 192.168.10.95:5000/calculator-div:latest .'
                }
            }
        }
        stage('Push images') {
            steps {
                sh '''
                    docker push 192.168.10.95:5000/calculator-calc:latest
                    docker push 192.168.10.95:5000/calculator-sum:latest
                    docker push 192.168.10.95:5000/calculator-sub:latest
                    docker push 192.168.10.95:5000/calculator-mul:latest
                    docker push 192.168.10.95:5000/calculator-div:latest
                '''
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -n default -f kubernetes/
                    for service in calc sum sub mul div; do
                        kubectl rollout status -n default deployment/calculator-$service --timeout=180s
                    done
                '''
            }
        }
    }

    post {
        success {
            echo 'Toutes les images ont été construites, publiées et déployées sur Kubernetes !'
        }
        failure {
            echo 'CI/CD : échec du pipeline.'
        }
    }
}

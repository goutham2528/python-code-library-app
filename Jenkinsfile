pipeline {
    agent any 
    stages {
        stage('Clean Workspace') {
            steps {
                sh 'docker stop auth_service book_service borrow_service || true'
                sh 'docker rm auth_service book_service borrow_service || true'
                sh 'docker rmi image1 image2 image3 || true'
            }
        }
        
        stage('Build All Services') {
            parallel {
                stage('Auth Build') {
                    steps {
                        dir('auth') {
                            sh 'docker build -t image1 .'
                        }
                    }
                }
                stage('Book Build') {
                    steps {
                        dir('book') {
                            sh 'docker build -t image2 .'
                        }
                    }
                }
                stage('Borrow Build') {
                    steps {
                        dir('borrow') {
                            sh 'docker build -t image3 .'
                        }
                    }
                }
            }
        }
        
        stage('Run Services') {
            steps {
                script {
                    // Stop any existing containers first
                    sh 'docker stop auth_service || true'
                    sh 'docker stop book_service || true'
                    sh 'docker stop borrow_service || true'
                    
                    // Remove any existing containers
                    sh 'docker rm auth_service || true'
                    sh 'docker rm book_service || true'
                    sh 'docker rm borrow_service || true'
                    
                    // Run new containers
                    sh 'docker run -d --name auth_service -p 5001:5001 image1'
                    sh 'docker run -d --name book_service -p 5002:5002 image2'
                    sh 'docker run -d --name borrow_service -p 5003:5003 image3'
                }
            }
        }
        
        stage('Verify Services') {
            steps {
                script {
                    // Wait for services to start and check health
                    sleep 10
                    sh 'docker ps'
                    sh 'curl -f http://localhost:5001/health || echo "Auth service health check failed"'
                    sh 'curl -f http://localhost:5002/health || echo "Book service health check failed"'
                    sh 'curl -f http://localhost:5003/health || echo "Borrow service health check failed"'
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed - cleaning up'
            sh 'docker stop auth_service book_service borrow_service || true'
            sh 'docker rm auth_service book_service borrow_service || true'
        }
        failure {
            echo 'Pipeline failed - sending notification'
            // Add notification logic here
        }
    }
}

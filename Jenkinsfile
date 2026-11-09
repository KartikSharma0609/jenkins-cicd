pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm myapp:${BUILD_NUMBER} sh -c "pip install -q pytest && pytest"'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker rm -f myapp || true'
                sh 'docker run -d --name myapp myapp:${BUILD_NUMBER}'
            }
        }
    }
}

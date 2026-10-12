pipeline {
    agent any

    stages {
        stage('Checkout Application') {
            steps {
                dir('py-app') {
                    git branch: "main",
                        url: "https://github.com/Vetrivel-G-work/flask-devsecops-demo.git"
                }
            }
        }

        stage('Prepare Build Context') {
            steps {
                sh '''
                    cp Dockerfile py-app/Dockerfile
                    echo "Build context prepared"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('py-app') {
                    sh '''
                        docker build \
                            -t flask-devsecops-demo:${BUILD_NUMBER} .
                    '''
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        APP_REPO = 'https://github.com/Vetrivel-G-work/flask-devsecops-demo.git'
        APP_BRANCH = 'main'
        IMAGE_NAME = 'flask-devsecops-demo'
        CONTAINER_NAME = 'flask-devsecops-demo'
        APP_PORT = '5000'
    }

    stages {
        stage('Checkout Application') {
            steps {
                dir('application') {
                    git branch: "main",
                        url: "https://github.com/Vetrivel-G-work/flask-devsecops-demo.git"
                }
            }
        }

        stage('Prepare Build Context') {
            steps {
                sh '''
                    cp Dockerfile application/Dockerfile
                    echo "Build context prepared"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('application') {
                    sh '''
                        docker build \
                            -t flask-devsecops-demo:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${APP_PORT}:5000 \
                        ${IMAGE_NAME}:${BUILD_NUMBER}

                    docker ps
                '''
            }
        }
    }
}

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
                    git branch: "${APP_BRANCH}",
                        url: "${APP_REPO}"
                }
            }
        }

        stage('Prepare Build Context') {
            steps {
                sh '''
                    test -f Dockerfile
                    cp Dockerfile application/Dockerfile
                    test -f application/app.py
                    test -f application/requirements.txt
                    echo "Build context prepared"
                '''
            }
        }

        stage('Validate Python') {
            steps {
                dir('application') {
                    sh '''
                        python3 --version
                        python3 -m py_compile app.py
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('application') {
                    sh '''
                        docker build \
                            -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                            -t ${IMAGE_NAME}:latest .
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

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

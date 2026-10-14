pipeline {
    agent any

    environment {
        IMAGE_NAME = 'edulab-frontend'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Frontend') {
            steps {
                sh '''
                    echo "Checking project files..."
                    ls -la

                    if [ ! -f index.html ]; then
                        echo "ERROR: index.html not found"
                        exit 1
                    fi

                    echo "Frontend validation successful"
                '''
            }
        }

        stage('Build DEV Docker Image') {
            when {
                branch 'develop'
            }

            steps {
                sh '''
                    echo "Building DEV Docker image..."

                    docker build \
                    -t ${IMAGE_NAME}:dev-${BUILD_NUMBER} \
                    .

                    docker images | grep ${IMAGE_NAME}
                '''
            }
        }

        stage('Production') {
            when {
                branch 'main'
            }

            steps {
                echo "Production pipeline will be added later"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully"
        }

        failure {
            echo "Pipeline failed"
        }
    }
}
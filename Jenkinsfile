pipeline {
    agent any

    environment {
        IMAGE_NAME = 'sanket007798/edulab-frontend'
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
                    echo "Checking frontend files..."
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
                    echo "Building DEV image..."

                    docker build \
                    -t ${IMAGE_NAME}:dev-${BUILD_NUMBER} \
                    .
                '''
            }
        }

        stage('Docker Hub Login') {
            when {
                branch 'develop'
            }

            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKERHUB_TOKEN'
                    )
                ]) {
                    sh '''
                        echo "$DOCKERHUB_TOKEN" | \
                        docker login \
                        -u "$DOCKERHUB_USERNAME" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push DEV Docker Image') {
            when {
                branch 'develop'
            }

            steps {
                sh '''
                    echo "Pushing DEV Docker image..."

                    docker push ${IMAGE_NAME}:dev-${BUILD_NUMBER}
                '''
            }
        }

        stage('Production') {
            when {
                branch 'main'
            }

            steps {
                echo "Production pipeline will be configured later"
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

        always {
            sh 'docker logout || true'
        }
    }
}
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Branch Information') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
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

        stage('DEV Pipeline') {
            when {
                branch 'develop'
            }

            steps {
                echo "DEV branch detected"
                echo "Later: Build Docker image and deploy to DEV Kubernetes"
            }
        }

        stage('PRODUCTION Pipeline') {
            when {
                branch 'main'
            }

            steps {
                echo "PRODUCTION branch detected"
                echo "Later: Build Docker image and deploy to Production Kubernetes"
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
pipeline {
    agent { label 'dev' }

    environment {
        APP_NAME = "demo-app"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                echo "Running on node: ${env.NODE_NAME}"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            when {
                not { branch 'main' }
            }
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to DEV') {
            when {
                branch pattern: "feature/.*", comparator: "REGEXP"
            }
            steps {
                echo "Deploying ${APP_NAME} to DEV environment"
            }
        }

        stage('Deploy to QA') {
            when {
                branch 'develop'
            }
            steps {
                echo "Deploying ${APP_NAME} to QA environment"
            }
        }

        stage('Approval for PROD') {
            when {
                branch 'main'
            }
            steps {
                input message: "Approve deployment to PROD?", ok: "Deploy"
            }
        }

        stage('Deploy to PROD') {
            when {
                branch 'main'
            }
            steps {
                echo "🚀 Deploying ${APP_NAME} to PROD environment"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS for ${env.BRANCH_NAME}"
        }
        failure {
            echo "Pipeline FAILED for ${env.BRANCH_NAME}"
        }
        always {
            echo "Pipeline finished for ${env.BRANCH_NAME}"
        }
    }
}

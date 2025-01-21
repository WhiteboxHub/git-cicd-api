pipeline {
    agent any

    environment {
        API_REPO_URL = 'https://github.com/WhiteboxHub/git-cicd-api.git'
        API_BUILD_DIR = 'api-build'
        DEPLOY_ENV = 'staging' // or 'production'
    }

    stages {
        stage('Checkout API') {
            steps {
                git url: "${API_REPO_URL}", branch: 'main'
            }
        }

        stage('Build API') {
            steps {
                dir("${API_BUILD_DIR}") {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test API') {
            steps {
                dir("${API_BUILD_DIR}") {
                    sh 'mvn test'
                }
            }
        }

        stage('Deploy API') {
            steps {
                script {
                    // Add your deployment steps here
                    // For example, copying the API JAR file to a deployment directory
                    sh "cp ${API_BUILD_DIR}/target/your-api.jar /path/to/deployment/directory/"
                    // Start the API application
                    sh "nohup java -jar /path/to/deployment/directory/your-api.jar > api.log 2>&1 &"
                }
            }
        }
    }

    post {
        always {
            echo 'API Pipeline completed.'
        }
        success {
            echo 'API Pipeline succeeded.'
            // Notify success
            sh 'curl -X POST -d "" your-success-webhook-url'
        }
        failure {
            echo 'API Pipeline failed.'
            // Notify failure
            sh 'curl -X POST -d "" your-failure-webhook-url'
        }
    }
}

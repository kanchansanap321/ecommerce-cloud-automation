pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Backend Build and Test') {
            steps {
                dir('eStoreBackend') {
                    script {
                        if (isUnix()) {
                            sh 'mvn clean test'
                        } else {
                            bat '"C:\\Users\\kanch\\Downloads\\apache-maven-3.9.16-bin\\apache-maven-3.9.16\\bin\\mvn.cmd" clean test'
                        }
                    }
                }
            }
        }

        stage('Frontend Install') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm install --legacy-peer-deps'
                    } else {
                        bat 'npm install --legacy-peer-deps'
                    }
                }
            }
        }

        stage('Frontend Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npx ng test --watch=false --browsers=ChromeHeadless'
                    } else {
                        bat 'npx ng test --watch=false --browsers=ChromeHeadless'
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker build -t ecommerce-frontend .'
                        sh 'docker build -t ecommerce-backend ./eStoreBackend'
                    } else {
                        bat 'docker build -t ecommerce-frontend .'
                        bat 'docker build -t ecommerce-backend ./eStoreBackend'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
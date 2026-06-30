pipeline {
    agent any

    environment {
        // Defines the Node.js installation configured in Jenkins Global Tool Configuration
        NODE_TOOL = 'NodeJS' 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Installs npm packages based on package-lock.json
                bat 'npm ci' 
            }
        }

        stage('Lint & Test') {
            steps {
                // Runs your test suite (CI=true prevents the watcher from hanging)
                bat 'set CI=true && npm test'
            }
        }

        stage('Build') {
            steps {
                // Compiles the React app into production-ready static files in the \build folder
                bat 'npm run build'
            }
        }

        stage('Deployment') {
            steps {
                // Clears the target web server directory
                bat 'del /q /s C:\\nginx\\html\\reactapp\\*'
                
                // Copies the compiled static build files to the web server
                bat 'xcopy /E /Y /I build\\* C:\\nginx\\html\\reactapp\\'
            }
        }
    }

    post {
        success {
            echo 'React Build, Test, and Deployment completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the logs for errors.'
        }
    }
}

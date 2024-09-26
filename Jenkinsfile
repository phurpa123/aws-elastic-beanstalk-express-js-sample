pipeline {
    agent {
        docker {
            image 'node:16'
            args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Check Environment and Diagnostics') {
            steps {
                echo 'Checking environment...'
                sh 'env'
                sh 'pwd'
                sh 'ls -al /var/jenkins_home/workspace'
                sh 'whoami'
            }
        }
        stage('Check Node.js and npm') {
            steps {
                echo 'Checking Node.js and npm...'
                sh 'node --version'
                sh 'npm --version'
            }
        }
        stage('Checkout') {
            steps {
                echo 'Checking out the code...'
                git url: 'https://github.com/phurpa123/aws-elastic-beanstalk-express-js-sample', branch: 'main'
            }
        }
        stage('Check for package.json') {
            steps {
                echo 'Checking for package.json...'
                sh 'ls -al package.json'
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install --save'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Running Snyk security scan...'
                sh 'npm install -g snyk'
                sh 'snyk test --severity-threshold=high'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'npm test'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Pipeline failed.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}

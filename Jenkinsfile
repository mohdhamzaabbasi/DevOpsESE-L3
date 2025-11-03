pipeline {
    agent any
    tools {
        jdk 'jdk'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/mohdhamzaabbasi/jenkins-auto-test.git', branch: 'main'
            }
        }
        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    bat 'npm install'
                }
            }
        }
        stage('Run Backend Tests') {
            steps {
                dir('backend') {
                    bat 'npm test'  // Runs Jest tests via package.json script
                }
            }
            
        }
        stage('Generate Backend Coverage') {
            steps {
                dir('backend') {
                    bat 'npm test -- --coverage'  // Generates coverage report
                }
            }
            post {
                always {
                    junit 'backend/reports/junit.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                bat '''
                curl.exe -sS -X POST ^
                -H "Accept: application/json" ^
                -H "Content-Type: application/json" ^
                "https://api.render.com/deploy/srv-d11csdm3jp1c73c9palg?key=dVm1pFJhYPc"

                IF ERRORLEVEL 1 EXIT /B 1
                '''
            }
        }
    }
}
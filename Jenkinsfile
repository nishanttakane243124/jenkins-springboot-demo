pipeline {

    agent any

    stages {

        stage('Test') {
            steps {
                echo 'Running Maven tests...'
                bat 'mvn clean test'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Spring Boot JAR...'
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Spring Boot application...'

                bat '''
                    for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8081 ^| findstr LISTENING') do (
                        taskkill /PID %%a /F
                    )

                    start "SpringBootApp" /B java -jar target\\jenkins-springboot-demo-0.0.1-SNAPSHOT.jar

                    powershell -Command "Start-Sleep -Seconds 10"
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying Spring Boot deployment...'

                bat 'curl -f http://localhost:8081/api/status'
            }
        }
    }

    post {

        success {
            echo '============================================'
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '============================================'
            echo 'Spring Boot application is running.'
            echo 'URL: http://localhost:8081'
        }

        failure {
            echo '============================================'
            echo 'CI/CD PIPELINE FAILED'
            echo '============================================'
        }
    }
}


        stage('Verify') {
            steps {
                echo 'Verifying Spring Boot deployment...'

                bat 'curl -f http://localhost:8081/api/status'
            }
        }
    }

    post {

        success {
            echo '============================================'
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '============================================'
            echo 'Spring Boot application is running.'
            echo 'URL: http://localhost:8081'
        }

        failure {
            echo '============================================'
            echo 'CI/CD PIPELINE FAILED'
            echo '============================================'
        }
    }
}
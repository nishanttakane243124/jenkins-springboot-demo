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
                    echo Checking for existing application...

                    for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8082 ^| findstr LISTENING') do (
                        echo Stopping PID %%a
                        taskkill /PID %%a /F
                    )

                    echo Starting new Spring Boot application...

                    start "SpringBootApp" /MIN cmd /c "set JENKINS_NODE_COOKIE=dontKillMe && java -jar target\\jenkins-springboot-demo-0.0.1-SNAPSHOT.jar"

                    powershell -Command "Start-Sleep -Seconds 15"

                    echo Application startup wait completed.
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying Spring Boot application...'

                bat '''
                    echo Checking port 8082...
                    netstat -ano | findstr :8082

                    echo Checking application endpoint...
                    curl -f http://localhost:8082/api/status
                '''
            }
        }
    }

    post {

        success {
            echo '============================================'
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '============================================'
            echo 'Application: http://localhost:8082'
        }

        failure {
            echo '============================================'
            echo 'CI/CD PIPELINE FAILED'
            echo '============================================'
        }
    }
}
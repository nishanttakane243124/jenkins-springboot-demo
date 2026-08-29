pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'Running Maven tests...'
                sh 'mvn clean test'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Spring Boot JAR...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Spring Boot application...'

                sh '''
                    pkill -f "jenkins-springboot-demo" || true

                    nohup java -jar target/jenkins-springboot-demo-0.0.1-SNAPSHOT.jar \
                    > springboot.log 2>&1 &

                    sleep 10
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Checking deployed application...'

                sh '''
                    curl -f http://localhost:8081/api/status
                '''
            }
        }
    }

    post {

        success {
            echo '===================================='
            echo 'CI/CD PIPELINE SUCCESSFUL'
            echo '===================================='
            echo 'Spring Boot application deployed.'
        }

        failure {
            echo '===================================='
            echo 'PIPELINE FAILED'
            echo '===================================='
        }
    }
}
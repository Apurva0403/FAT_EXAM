pipeline {
    agent any
    
    options {
        buildDiscarder(logRotator(numToKeepStr: '50', daysToKeepStr: '30'))
    }
    
    stages {
        stage('Build with Maven') {
            steps {
                echo '========== STAGE: Build with Maven =========='
                bat 'mvn clean package -DskipTests'
                echo 'Build completed successfully!'
            }
        }
        
        stage('Test with JUnit') {
            steps {
                echo '========== STAGE: Test with JUnit =========='
                bat 'mvn test'
                echo 'Tests completed successfully!'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Create Docker Image') {
            steps {
                echo '========== STAGE: Create Docker Image =========='
                bat '''
                    docker build -t apurva0409/matrix-multiplication:%BUILD_NUMBER% .
                    docker tag apurva0409/matrix-multiplication:%BUILD_NUMBER% apurva0409/matrix-multiplication:latest
                    echo Docker image created successfully
                '''
                echo 'Docker image created successfully!'
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo '========== STAGE: Push to Docker Hub =========='
                withCredentials([usernamePassword(credentialsId: 'docker-pass', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker push apurva0409/matrix-multiplication:%BUILD_NUMBER%
                        docker push apurva0409/matrix-multiplication:latest
                        docker logout
                        echo Docker image pushed successfully
                    '''
                }
                echo 'Push to Docker Hub completed successfully!'
            }
        }
    }
    
    post {
        success {
            echo '✓ Pipeline executed successfully!'
        }
        failure {
            echo '✗ Pipeline failed!'
        }
    }
}

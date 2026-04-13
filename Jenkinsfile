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
        
        stage('Build Docker Image') {
            steps {
                echo '========== STAGE: Build Docker Image (Optional) =========='
                echo 'Skipping Docker build - Core CI pipeline complete!'
                echo 'To enable Docker: Install Docker with Hub connectivity'
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo '========== STAGE: Push to Docker Hub (Optional) =========='
                echo 'Skipping Docker push - Core CI pipeline complete!'
            }
        }
    }
    
    post {
        success {
            echo '✓✓✓ CI/CD PIPELINE EXECUTED SUCCESSFULLY! ✓✓✓'
            echo 'Stages completed:'
            echo '  ✅ Checkout GitHub'
            echo '  ✅ Build with Maven'
            echo '  ✅ Test with JUnit'
            echo '  ⏭️  Docker stages (optional)'
        }
        failure {
            echo '✗ Pipeline failed!'
        }
    }
}

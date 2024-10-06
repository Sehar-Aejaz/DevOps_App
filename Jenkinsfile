pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
    }
    
    tools {
        nodejs "NodeJS_14" 
    }

    stages {
        // Stage 1: Checkout code from Git
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sehar-Aejaz/Jenkins-HD'
            }
        }

        // Stage 2: Install dependencies
        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        
        // Stage 3: Install supertest dependency
        stage('Install dependencies') {
            steps {
                sh 'npm install supertest --save-dev'
            }
        }

        // Stage 4: Run tests using Jest
        stage('Test') {
            steps {
                sh 'npm install jest --save-dev'  
                script {
                    sh 'npm test'
                }
            }
        }

        // Stage 5: Deploy to a test environment
        stage('Deploy to Test Environment') {
            steps {
                script {
                  
                    sh '''
                        docker build -t Jenkins-HD:test .
                        docker run -d -p 3000:3000 Jenkins-HD:test
                    '''
                    
                }
            }
        }

        // Stage 6: Release to production 
        stage('Release to Production') {
            steps {
                script {
                    // Release to production environment
                    sh '''
                        docker tag Jenkins-HD:test Jenkins-HD:latest
                        docker push Jenkins-HD:latest
                    '''
                    
                }
            }
        }
    }

    post {
        success {
            emailext subject: "Pipeline '${currentBuild.fullDisplayName}' Successful",
                      body: 'The build was successful. Congratulations!',
                      to: 'seharaejaz4@gmail.com'
        }
        failure {
            attachLog: true
            emailext subject: "Pipeline '${currentBuild.fullDisplayName}' Failed",
                      body: 'The build has failed. Please investigate.',
                      to: 'seharaejaz4@gmail.com',
                      attachLog: true
        }
    }

}

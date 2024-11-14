pipeline {
    agent any

    stages {
        // This is Build Stage
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        // This is Test Stage
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        // This is E2E Stage
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.48.1-noble'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

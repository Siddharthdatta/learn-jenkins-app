pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm ci
                    npm run build
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.59.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm ci
                    npm install -g serve

                    echo "Starting app on port 3000..."
                    serve -s build -l 3000 &

                    echo "Waiting for server..."
                    npx wait-on http://localhost:3000

                    echo "Running Playwright tests..."
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results/**/*.xml'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'playwright-report/**'
        }
    }
}
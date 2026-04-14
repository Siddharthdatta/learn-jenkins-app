pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
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
                ls -la
                '''
            }
        }
        /*stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
              
              steps {
               // sh '''
               // test -f build/index.html
                // npm test 
                  // '''
              }
        }*/
        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.58.2-noble'
                    reuseNode true
                    
                }
            }
              
              steps {
                sh '''
                    npx install -g serve 
                    node_modules\.bin\serve -s build
                    npx playwright tes 
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
  
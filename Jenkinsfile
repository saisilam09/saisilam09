pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                
                sh'''
                 ls -la
                 npm --version
                 node --version
                 npm ci
                 npm run build
                 ls -la
                '''
            }
        }
        stage('test'){
              agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
              }
            steps{
                sh '''
                  test -f build/index.html
                  npm test
                '''
                
            }

        }
        stage('E2E'){
              agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.62.0-noble'
                    reuseNode true
                }
              }
            steps{
                sh '''
                 npm install -g serve
                 node-modules/.bin/serve -s build $
                 seelp 10
                 npx playwrite test
                '''
                
            }

        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
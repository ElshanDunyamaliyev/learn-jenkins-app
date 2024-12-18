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
        stage('Test'){
          agent{
            docker{
              image 'node:18-alpine'
              reuseNode true
            }
          }
          steps{
            // Just a comment, if we use # inside sh it will comment the line and pass it to the next command
            sh '''
              #test -f ./build/index.html
              npm ci
              npm test
            '''
          }
        }
         stage('E2E'){
          agent{
            docker{
              image 'mcr.microsoft.com/playwright:v1.49.1-noble'
              reuseNode true
            }
          }
          steps{
            sh '''
                npm install serve
                node_modules\\serve -s build &
                sleep 10
                npx playwright test
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

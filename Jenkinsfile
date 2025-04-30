pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '1848a6f7-82ce-493d-bbec-9c8ee83402f2'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
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
         stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                 sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploys') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                 sh '''
                   npm install netlify-cli
                   node_modules/.bin/netlify --version

                '''
            }
        }
    }
    post {
        always {
            junit 'test-result/junit.xml'
        }
    }
}

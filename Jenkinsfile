pipeline {
    agent any

    stages {
        stage ('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true  //reuse the same container for next subsequent stages
                }
            }
            steps {
                sh '''
                    ls -l
                    node -v
                    npm -v
                    npm config set cache ${PWD}/.npm-cache
                    npm install
                    npm run build
                    ls -l
                '''
            }
        }
    }
}
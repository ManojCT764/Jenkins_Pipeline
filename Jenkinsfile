pipeline {
    agent any

    stages {

        stage('cleanup') {
            steps {
                cleanWs() //clean up the workspace before doing anything
            }
        }
        stage ('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true  //reuse the same container for next subsequent stages
                }
            }
            
            steps {
                sh '''
                    ls -l
                    node -v
                    npm -v
                    mkdir -p ${PWD}/.npm-cache
                    npm install --cache=${PWD}/.npm-cache
                    npm run build --cache=${PWD}/.npm-cache
                    ls -l
                '''
            }
        }
    }
}
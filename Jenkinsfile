pipeline {
    agent any

    options {
        skipDefaultCheckout() //skip the default checkout of source code
        timestamps() //add timestamps to the console output
    }
    stages {

        stage('cleanup') {
            steps {
                cleanWs() //clean up the workspace before doing anything
            }
        }

        stage('Checkout') {
            steps {
                checkout scm //checkout the source code from the configured SCM
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

        stage('Test') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true  //reuse the same container for next subsequent stages
                }
            }

            steps {
                sh '''
                    ls -l
                    npm test --cache=${PWD}/.npm-cache
                '''
            }
        }
    }
}
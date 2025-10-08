pipeline {
    agent any

    environment {
        my_var = 'Hello, Jenkins!'
        VERCEL_TOKEN = credentials('REACT_JENKINS_PIPELINE')
    }
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

        stage('Get approvals') {
            steps{
                // we acn also add a timeout to avoid waiting indefinitely
                timeout(time: 1, unit: 'HOURS') {
                    input message: 'Do you want to proceed to deployment?', ok: 'Yes, proceed'
                }   
            }
        }

        stage ('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    args '-u root:root' //run the container as root user to avoid permission issues
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
                    args '-u root:root' //run the container as root user to avoid permission issues
                    reuseNode true  //reuse the same container for next subsequent stages
                }
            }

            steps {
                sh '''
                    npm run test --cache=${PWD}/.npm-cache
                '''
            }
        }

        stage('Deploy') {

            agent {
                docker {
                    image 'node:20-alpine'
                    args '-u root:root' //run the container as root user to avoid permission issues
                    reuseNode true  //reuse the same container for next subsequent stages
                }
            }

            steps {
                sh '''
                    npm install -g vercel
                    echo $my_var
                    vercel --prod --token=$VERCEL_TOKEN --confirm --name=jenkins-pipeline
                '''
                // REACT_JENKINS_PIPELINE
                //  VIQxtXagVddlBACUrlnwvryD 
            }
        }
    }
}
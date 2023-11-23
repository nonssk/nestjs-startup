pipeline {
    agent any

    tools {
        nodejs 'node_18_16_0'
    }

    stages {
        stage('Install') {
            steps {
                script {
                    sh 'yarn install'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'yarn test'
                }
            }
        }
        stage('build') {
            steps {
                script {
                     def name = sh(script: 'node -pe "require(\'./package.json\').name"', returnStdout: true).trim()
                    def version = sh(script: 'node -pe "require(\'./package.json\').version"', returnStdout: true).trim()
                    env.NAME = name
                    env.VERSION = version
                    echo "Build project ${env.NAME} version ${env.VERSION}"
                    sh 'cp .env.prod .env'
                    sh "docker build -t ${env.NAME} ."
                }
            }
        }
        stage('public') {
            steps {
                script {
                    sh "docker login -u nonssk403 -p dckr_pat_kNNn2pcFGwxRcjB-2mYDili0I8s"
                    sh "docker tag ${env.NAME} nonssk403/${env.NAME}:${env.VERSION}"
                    sh "docker push nonssk403/${env.NAME}:${env.VERSION}"
                    sh "docker rmi ${env.NAME}"
                    sh "docker rmi nonssk403/${env.NAME}:${env.VERSION}"
                }
            }
        }
    }
}


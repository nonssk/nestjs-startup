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
        stage('Setup ENV') {
            steps {
                script {
                    def name = sh(script: 'node -pe "require(\'./package.json\').name"', returnStdout: true).trim()
                    def version = sh(script: 'node -pe "require(\'./package.json\').version"', returnStdout: true).trim()
                    env.NAME = name
                    env.VERSION = version
                    env.DOCKER_HUB = "nonssk403"
                    env.DOCKER_HUB_TOKEN = "dckr_pat_kNNn2pcFGwxRcjB-2mYDili0I8s"
                    // def yamlFilePath = 'deploy.yml'
                    // def yamlContent = readFile(file: yamlFilePath).trim()
                    // def yamlData = readYaml text: yamlContent
                    // yamlData.spec.template.spec.containers[0].image = "${env.DOCKER_HUB}/${env.NAME}:${env.VERSION}"
                    // def modifiedYamlContent = writeYaml yamlData
                    // writeFile file: yamlFilePath, text: modifiedYamlContent
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo "Build project ${env.NAME} version ${env.VERSION}"
                    sh 'cp .env.prod .env'
                    sh "docker build -t ${env.NAME} ."
                }
            }
        }
        stage('Public') {
            steps {
                script {
                    sh "docker login -u ${env.DOCKER_HUB} -p ${env.DOCKER_HUB_TOKEN}"
                    sh "docker tag ${env.NAME} ${env.DOCKER_HUB}/${env.NAME}:${env.VERSION}"
                    sh "docker push ${env.DOCKER_HUB}/${env.NAME}:${env.VERSION}"
                    sh "docker rmi ${env.NAME}"
                    sh "docker rmi ${env.DOCKER_HUB}/${env.NAME}:${env.VERSION}"
                }
            }
        }
        stage('Rollout') {
            steps {
                script {
                    echo "Rollout Success"
                }
            }
        }
    }
}


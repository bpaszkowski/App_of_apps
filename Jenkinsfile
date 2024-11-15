def frontendImage="thatgreendragon/panda-frontend"
def backendImage="thatgreendragon/panda-backend"


properties([parameters([string(defaultValue: 'latest', name: 'backendDockerTag'), string(defaultValue: 'latest', name: 'frontendDockerTag')])])

pipeline {
    agent {
        label 'agent'
    }
    stages {
        stage('Get Code!') {
            steps {
                checkout scm
            }
        }
        stage('Adjust version'){
            steps {
                script {
                currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
        }

        stage('Clean running containers') {
            steps {
                    sh "docker rm -f frontend backend"
                    }
}

        stage('Deploy applcation'){
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                            sh "docker-compose up -d"
                }
            }
        }
    }

        post {
            always {
                    sh "docker-compose down"
                    cleanWs()
                }
            }

    }
}
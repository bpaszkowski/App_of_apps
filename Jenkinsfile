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
    }
}
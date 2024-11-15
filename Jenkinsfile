def frontendImage="thatgreendragon/panda-frontend"
def backendImage="thatgreendragon/panda-backend"


properties([parameters([string(defaultValue: 'latest', name: 'backendDockerTag'), string(defaultValue: 'latest', name: 'frontendDockerTag')])])

pipeline {
    agent {
        label 'agent'
    }

    environment {
        PIP_BREAK_SYSTEM_PACKAGES = 1
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

        stage('Deploy application'){
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                            sh "docker-compose up -d"
                }
            }
        }
    }
            stage('Selenium tests') {
                steps {
                    sh "pip3 install -r test/selenium/requirements.txt"
                    sh "python3 -m pytest test/selenium/frontendTest.py"
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
pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        appVersion = ""
    }
    stages {
        stage('ReadJsonVersion') {
            steps {
                script {
                    def PackageJSON = readJSON file: 'package.json'
                    appVersion = PackageJSON.version
                    echo "appversion: ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh """
                        ls -lart
                        docker build -t catalogue:${appVersion} .
                        docker images
                    """
                }
            }
        }

    }
}
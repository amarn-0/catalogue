pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        appVersion = ""
        ACC_ID = "160932097178"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
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

        stage("unit test") {
            steps {
                script{
                    sh """
                        npm test
                    """
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    withAWS( credentials: 'access-key', region: 'us-east-1'){
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker images
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                    }
                }
            }
        }

    }
}
pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        appVersion = ""
        acc_id = "160932097178"
        project = "roboshop"
        component = "catalogue"
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
                    withAWS( credentials: 'access-key', region: 'us-east-1'){
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${acc_id}.dkr.ecr.us-east-1.amazonaws.com
                            docker build ${acc_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                            docker images
                            docker push ${acc_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                        """
                    }
                }
            }
        }

    }
}
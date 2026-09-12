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
            script {
                def PackageJSON = readJSON file: 'package.json'
                appVersion = PackageJSON.version
                echo "appversion: ${appVersion}"
            }
        }
    }
}
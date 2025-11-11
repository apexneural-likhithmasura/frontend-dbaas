pipeline {
    agent any

    environment {
        NODE_HOME = "D:\\"
        PATH = "${NODE_HOME};${env.PATH}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: "*/dev"]],
                    userRemoteConfigs: [[
                        url: 'https://github.com/apexneural-likhithmasura/frontend-dbaas.git',
                        credentialsId: 'github-token'
                    ]]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                bat """
                    D:\\npm.cmd install
                """
            }
        }

        stage('Build Frontend') {
            steps {
                bat """
                    D:\\npm.cmd run build
                """
            }
        }

        stage('Zip Build') {
            steps {
                bat """
                    "C:\\Program Files\\7-Zip\\7z.exe" a build.zip .\\dist\\*
                """
            }
        }

        stage('Deploy to WSL') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'wsl-ssh',      // <-- Your Jenkins SSH Credential ID
                        transfers: [
                            sshTransfer(
                                sourceFiles: 'build.zip',
                                remoteDirectory: '/var/www/frontend',
                                removePrefix: '',
                                execCommand: '''
                                    cd /var/www/frontend
                                    rm -rf *
                                    unzip build.zip
                                '''
                            )
                        ]
                    )
                ])
            }
        }
    }
}

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
                        configName: 'wsl-ssh',
                        transfers: [
                            sshTransfer(
                                sourceFiles: 'build.zip',
                                remoteDirectory: '/var/www',
                                removePrefix: '',
                                execCommand: '''
                                    cd /var/www
                                    rm -rf frontend
                                    mkdir -p frontend
                                    unzip -o build.zip -d frontend
                                    sudo chown -R www-data:www-data frontend
                                    sudo chmod -R 755 frontend
                                    rm -f build.zip
                                    echo "Deployment completed successfully"
                                    ls -la frontend
                                '''
                            )
                        ],
                        verbose: true
                    )
                ])
            }
        }
    }
    
    post {
        success {
            echo 'Frontend deployment completed successfully!'
        }
        failure {
            echo 'Frontend deployment failed. Check the logs above.'
        }
        always {
            cleanWs()
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/apexneural-likhithmasura/frontend-dbaas.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                cd updated-frontend
                D:\\npm.cmd install
                '''
            }
        }

        stage('Build Frontend') {
            steps {
                bat '''
                cd updated-frontend
                D:\\npm.cmd run build
                '''
            }
        }

        stage('Zip Build') {
            steps {
                bat '''
                "C:\\Program Files\\7-Zip\\7z.exe" a build.zip .\\updated-frontend\\dist\\*
                '''
            }
        }

        stage('Deploy to WSL') {
            steps {
                bat '''
                del /q "\\\\wsl$\\Ubuntu\\var\\www\\frontend\\*"
                "C:\\Program Files\\7-Zip\\7z.exe" x build.zip -o"\\\\wsl$\\Ubuntu\\var\\www\\frontend" -y
                '''
            }
        }
    }
}

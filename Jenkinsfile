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
                bat 'D:\\Fidget\\cmd\\git.exe clone https://github.com/apexneuralecosystems/updated-frontend.git frontend'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'cd frontend && D:\\npm.cmd install'
            }
        }

        stage('Build Frontend') {
            steps {
                bat 'cd frontend && D:\\npm.cmd run build'
            }
        }

        stage('Zip Build') {
            steps {
                bat '"C:\\Program Files\\7-Zip\\7z.exe" a D:\\deploy_temp\\build.zip .\\frontend\\dist\\*"'
            }
        }

        stage('Deploy to Windows Folder') {
            steps {
                bat 'if exist "D:\\frontend deployment" (rmdir /S /Q "D:\\frontend deployment")'
                bat 'mkdir "D:\\frontend deployment"'
                bat '"C:\\Program Files\\7-Zip\\7z.exe" x D:\\deploy_temp\\build.zip -o"D:\\frontend deployment" -y'
            }
        }
    }
}

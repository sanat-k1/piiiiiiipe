pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'python -m unittest discover -s .'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat '''
                if not exist "%WORKSPACE%\\python-app-deploy" mkdir "%WORKSPACE%\\python-app-deploy"
                copy "%WORKSPACE%\\app.py" "%WORKSPACE%\\python-app-deploy\\"
                '''
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application...'
                bat '''
                start /B python "%WORKSPACE%\\python-app-deploy\\app.py" > "%WORKSPACE%\\python-app-deploy\\app.log" 2>&1
                powershell -Command "(Get-Process -Name python).Id | Out-File -FilePath '%WORKSPACE%\\python-app-deploy\\app.pid'"
                '''
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing application...'
                bat '''
                python "%WORKSPACE%\\test_app.py"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}

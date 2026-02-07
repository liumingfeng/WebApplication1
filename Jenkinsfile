pipeline {
    agent any
    environment {
        DOTNET_VERSION = '10.0'  
        PROJECT_FILE = 'WebApplication1.slnx' 
        PUBLISH_DIR = '.\\publish' 
    }
    stages {
        stage('Clean up code') {
            steps {
                echo 'clean up working folder old files...'
                bat 'if exist %PUBLISH_DIR% rd /s /q %PUBLISH_DIR%'
            }
        }

        stage('Restore NuGet') {
            steps {
                echo 'start Restore NuGet...'
                bat "dotnet restore ${PROJECT_FILE} --verbosity normal"
            }
        }

        stage('build project') {
            steps {
                echo 'start build project'
                bat "dotnet build ${PROJECT_FILE} -c Release --no-restore"
            }
        }

        stage('publish project') {
            steps {
                echo 'start publish project'
                bat "dotnet publish ${PROJECT_FILE} -c Release -o ${PUBLISH_DIR} --no-build"
                
                echo 'publish finished'
                bat 'dir %PUBLISH_DIR%'
            }
        }
    }

    post {
        success {
            echo "Success .NET Core published to ${WORKSPACE}\\${PUBLISH_DIR}"
        }
        failure {
            echo "Failed please see logs"
        }
    }
}
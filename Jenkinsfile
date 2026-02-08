pipeline {
    agent any
    environment {
        PROJECT_NAME = "WebApplication1"  
        PUBLISH_DIR = "${WORKSPACE}\\publish"
        WSL_DISTRO_NAME = "Ubuntu"
    }

    stages {

        stage('restore packages') {
            steps {
                bat "dotnet restore ${PROJECT_NAME}.csproj"
            }
        }

        stage('build code') {
            steps {
                bat "rd /s /q ${PUBLISH_DIR} || echo PUBLISH_DIR_NOT_EXIST"
                bat "dotnet publish ${PROJECT_NAME}.csproj -c Release -o ${PUBLISH_DIR}"
            }
        }

        stage('Ansible deploy to IIS') {
            steps {
                bat """
                    wsl --distribution ${WSL_DISTRO_NAME} --exec echo "WSL started"
                    wsl --distribution ${WSL_DISTRO_NAME} ansible-playbook /mnt/c/ansible/deploy_iis.yml
                """
            }
        }
    }

    post {
        success {
            echo "Deploy success: http://localhost:81/WeatherForecast"
        }
        failure {
            echo "Deploy failed"
        }
    }
}
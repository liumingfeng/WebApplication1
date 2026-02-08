pipeline {
    agent any
    environment {
        PROJECT_NAME = "WebApplication1"  
        PUBLISH_DIR = "${WORKSPACE}\\publish"  
    }

    stages {

        stage('restore packages') {
            steps {
                bat "dotnet restore ${PROJECT_NAME}.csproj"
            }
        }

        stage('build code') {
            steps {
                bat "dotnet publish ${PROJECT_NAME}.csproj -c Release -o ${PUBLISH_DIR}"
            }
        }

        stage('Ansible deploy to IIS') {
            steps {
                bat """
                    # 先确保WSL启动
                    wsl --distribution Ubuntu-22.04 --exec echo "WSL started"
                    # 执行Ansible剧本
                    wsl ansible-playbook /mnt/c/ansible/deploy_iis.yml
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
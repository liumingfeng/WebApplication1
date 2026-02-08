pipeline {
    agent any
    environment {
        PROJECT_NAME = "WebApplication1"  
        PUBLISH_DIR = "${WORKSPACE}\\publish"  
    }

    stages {
        stage('pull code') {
            steps {
                git url: "https://github.com/liumingfeng/WebApplication1.git", branch: "master"
            }
        }

        stage('restore code') {
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
                bat "wsl ansible-playbook /mnt/c/ansible/deploy_iis.yml"
            }
        }
    }

    post {
        success {
            echo "Deploy success"
        }
        failure {
            echo "Deploy failed"
        }
    }
}
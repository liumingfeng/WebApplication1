pipeline {
    agent any
    environment {
        // ====================== 你只改这里 2 个 ======================
        PROJECT_NAME = "WebApplication1"  // 你的ASP.NET Core项目名（csproj文件名）
        PUBLISH_DIR = "${WORKSPACE}\\publish"  // Jenkins发布目录（自动生成）
        // ==========================================================
    }

    stages {
        // 1. 拉取代码（本地Git/远程Git都可）
        stage('拉取代码') {
            steps {
                // 本地Git示例（替换成你的Git地址）
                git url: "https://github.com/liumingfeng/WebApplication1.git", branch: "master"
            }
        }

        // 2. 还原NuGet依赖
        stage('还原依赖') {
            steps {
                bat "dotnet restore ${PROJECT_NAME}.csproj"
            }
        }

        // 3. 编译并发布
        stage('编译发布') {
            steps {
                bat "dotnet publish ${PROJECT_NAME}.csproj -c Release -o ${PUBLISH_DIR}"
            }
        }

        // 4. 调用WSL2里的Ansible部署到IIS
        stage('Ansible部署到IIS') {
            steps {
                // 核心：调用WSL2中的Ansible执行剧本
                bat "wsl ansible-playbook /mnt/c/ansible/deploy_iis.yml"
            }
        }
    }

    // 部署结果提示
    post {
        success {
            echo "✅ 部署成功！可访问 http://localhost:81/WebApplication1/WeatherForecast"
        }
        failure {
            echo "❌ 部署失败！请检查日志"
        }
    }
}
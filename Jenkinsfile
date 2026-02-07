pipeline {
    agent any
    environment {
        DOTNET_VERSION = '10.0'  // 替换为你的.NET版本（6/7/8）
        PROJECT_FILE = 'WebApplication1.csproj'  // 你的项目解决方案名（从Git地址看是WebApplication1）
        PUBLISH_DIR = '.\\publish'  // Windows路径用\\，也可直接用/
    }
    stages {
        stage('拉取代码') {
            steps {
                echo '清理工作空间旧文件...'
                // Windows批处理命令：清空当前工作空间（替代Linux的rm -rf *）
                bat 'del /f /s /q *.* && for /d %%i in (*) do rd /s /q %%i'
                
                echo '开始拉取Git仓库中的.NET Core项目代码...'
                
                echo '拉取代码完成（由Jenkinsfile托管）'
                // 拉取你的GitHub仓库代码（已适配你的仓库地址）
                //git url: 'https://github.com/liumingfeng/WebApplication1.git', 
                //    branch: 'master'  // 你的仓库分支是master（日志中显示refs/remotes/origin/master）
                    // credentialsId: 'git-cred'  // 若仓库私有，取消注释并填凭证ID
            }
        }

        stage('还原NuGet依赖') {
            steps {
                echo '开始还原项目NuGet依赖包...'
                // Windows用bat执行dotnet命令
                bat "dotnet restore ${PROJECT_FILE} --verbosity normal"
            }
        }

        stage('编译项目') {
            steps {
                echo '开始编译.NET Core项目...'
                bat "dotnet build ${PROJECT_FILE} -c Release --no-restore"
            }
        }

        stage('发布项目') {
            steps {
                echo '开始发布.NET Core项目到指定目录...'
                bat "dotnet publish ${PROJECT_FILE} -c Release -o ${PUBLISH_DIR} --no-build"
                
                echo '发布完成，查看产物列表：'
                // Windows查看目录文件的命令
                bat 'dir %PUBLISH_DIR%'
            }
        }
    }

    post {
        success {
            echo "✅ .NET Core项目构建发布成功！产物路径：${WORKSPACE}\\${PUBLISH_DIR}"
        }
        failure {
            echo "❌ .NET Core项目构建发布失败！请查看控制台日志排查问题"
        }
    }
}
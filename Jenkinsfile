pipeline {
    agent any
    parameters {
        string(name: 'GIT_HTTPS_PATH', defaultValue: 'https://github.com/tavisca-vshah/WebApiTests.git')
        string(name: 'TEST_FILE_PATH', defaultValue: 'WebApi.Tests/WebApi.Tests.csproj')
        string(name: 'SOLUTION_FILE_PATH', defaultValue: 'WebApi/WebApi.sln')
  }

    stages {
        stage('Build') {
            steps {
                powershell '''
              
                echo "----------------------------Build Project Started-----------------------------"
                dotnet build $($ENV:SOLUTION_FILE_PATH) -p:Configuration=release -v:n
                echo "----------------------------Build Project Completed-----------------------------"
               
                echo "----------------------------Test Project Started-----------------------------"
                dotnet test $($ENV:TEST_FILE_PATH)
                echo "----------------------------Test Project Completed-----------------------------"
               
                echo "----------------------------Publishing Project Started-----------------------------"
                dotnet publish $($ENV:SOLUTION_PATH) -c Release
                echo "----------------------------Publishing Project Completed-----------------------------"
               
                echo "----------------------------Docker Image Started-----------------------------"
                docker build --tag=pipe .
                echo "----------------------------Docker Image Completed-----------------------------"
                '''
            }
            }
               
        stage('Deploy') {
            steps {
                powershell '''
                echo "----------------------------Deploying Project Started-----------------------------"
                docker run -d -p 2000:80 pipe
                echo "----------------------------Deploying Project Completed-----------------------------"
                '''
            }
        }
    }
}
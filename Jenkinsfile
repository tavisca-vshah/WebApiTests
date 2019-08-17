pipeline {
    agent any
    parameters {
        string(name: 'GIT_HTTPS_PATH', defaultValue: 'https://github.com/tavisca-vshah/WebApiTests.git')
        string(name: 'TEST_FILE_PATH', defaultValue: 'WebApi.Tests/WebApi.Tests.csproj')
        string(name: 'SOLUTION_FILE_PATH', defaultValue: 'WebApi/WebApi.sln')
        string(name: 'Docker_IMAGE_TAG', defaultValue: '')
        string(name: 'PORT_NO', defaultValue: '')

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
                dotnet publish $($ENV:SOLUTION_FILE_PATH) -c Release
                echo "----------------------------Publishing Project Completed-----------------------------"
               
                echo "----------------------------Docker Image Started-----------------------------"
                docker build --tag=$($ENV:Docker_IMAGE_TAG) .
                echo "----------------------------Docker Image Completed-----------------------------"
                '''
            }
            }
               
        stage('Deploy') {
            steps {
                powershell '''
                echo "----------------------------Deploying Project Started-----------------------------"
                echo " run -p $($ENV:PORT_NO):80 $($ENV:Docker_IMAGE_TAG):latest"
                echo "----------------------------Deploying Project Completed-----------------------------"
                '''
            }
        }
    }
}
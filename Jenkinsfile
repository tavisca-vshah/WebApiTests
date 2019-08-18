pipeline {
    agent any
    parameters {
        string(name: 'GIT_HTTPS_PATH', defaultValue: 'https://github.com/tavisca-vshah/WebApiTests.git')
        string(name: 'TEST_FILE_PATH', defaultValue: 'WebApi.Tests/WebApi.Tests.csproj')
        string(name: 'SOLUTION_FILE_PATH', defaultValue: 'WebApi/WebApi.sln')
        string(name: 'SOLUTION_NAME', defaultValue: 'WebApi')
        string(name: 'DOCKER_USERNAME', defaultValue: 'vshahks4578')
        string(name: 'DOCKER_PASSWORD');
        string(name: 'DOCKER_REPO_NAME')
        

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
                dotnet publish $($ENV:SOLUTION_FILE_PATH) -c Release -o ../publish
                echo "----------------------------Publishing Project Completed-----------------------------"
               
                echo "----------------------------Docker Image Started-----------------------------"
                docker build --tag=$($ENV:DOCKER_USERNAME)/$($ENV:DOCKER_REPO_NAME) --build-arg project_name=$($ENV:SOLUTION_NAME).dll .
                echo "----------------------------Docker Image Completed-----------------------------"
                '''
            }
            }
               
        stage('Deploy') {
            steps {
                powershell '''
                echo "----------------------------Deploying Project Started-----------------------------"
                docker login -u $($ENV:DOCKER_USERNAME) -p $($ENV:DOCKER_PASSWORD)
                echo "docker push $($ENV:DOCKER_USERNAME)/$($ENV:DOCKER_REPO_NAME):v2"
                echo "----------------------------Deploying Project Completed-----------------------------"
                '''
            }
        }
    }
}
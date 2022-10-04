pipeline {
    agent any
     stages {
        stage('Restore packages'){
           steps{
               sh 'dotnet restore ${workspace}\\TestSwaggerApi\\TestSwaggerApi.sln'
            }
         }
        stage('Clean'){
           steps{
               sh 'dotnetClean TestSwaggerApi\\TestSwaggerApi.sln --configuration Release'
            }
         }
        stage('Build'){
           steps{
               sh 'dotnetBuild  TestSwaggerApi\\TestSwaggerApi.sln --configuration Release --no-restore'
            }
         }
         stage('Generate Docs'){
           steps{
              sh 'dotnet tool install SwashBuckle.AspNetCore.Cli'
              sh 'dotnet swagger tofile --output TestSwaggerApi.yaml --yaml .\\bin\\Release\\net6.0\\TestSwaggerApi.dll v1'
              sh 'cat TestSwaggerApi.yaml'
            }
         }
        stage('Publish'){
             steps{
               sh 'dotnet publish TestSwaggerApi\\TestSwaggerApi.csproj --configuration Release --no-restore'
             }
        }
        stage('Deploy'){
             steps{
               sh '''for pid in $(lsof -t -i:9090); do
                       kill -9 $pid
               done'''
               sh 'cd WebApplication/bin/Release/netcoreapp6/publish/'
               sh 'nohup dotnet WebApplication.dll --urls="http://aspcore.local:8092" --ip="127.0.0.1" --port=8092 --no-restore > /dev/null 2>&1 &'
             }
        }
    }
}
pipeline {
    agent any
     stages {
        stage('Restore packages'){
           steps{
               sh 'dotnet restore Webapptest.sln'
            }
         }
        stage('Clean'){
           steps{
               sh 'dotnet clean Webapptest.sln --configuration Release'
            }
         }
        stage('Build'){
           steps{
               sh 'dotnet build Webapptest.sln --configuration Release --no-restore'
            }
         }
        stage('Publish'){
             steps{
               sh 'dotnet publish Webapptest/Webapptest.csproj --configuration Release --no-restore'
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
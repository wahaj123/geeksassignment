pipeline {
    agent any
    stages {
        stage('git') {
            steps {
                git 'https://github.com/wahaj123/geeksassignment.git'
            }
        }
        stage('Restore packages'){
            steps{
                bat 'C:/ProgramData/Jenkins/.jenkins/workspace/pipeline_git/FirstApplication/FirstApplication.csproj'
            }
        }
        stage('Build'){
            steps{
                bat "\"${tool 'MSBuild'}\" FirstApplication.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=devops /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:publishUrl=c:\\inetpub\\wwwroot\\devops"
    		}
        }              
        stage('clearing'){
            steps{
                bat 'dotnet nuget locals temp -c'
            }
        }
    }
}

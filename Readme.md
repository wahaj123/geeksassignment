# Using Jenkins Pipeline to Deploy ASP.NET CORE Application on IIS

In this Documentation we will host ASP.NET CORE Application on IIS server and will then use jenkins pipeline to deploy it.

Follow the complete steps to get it start working.
1. Create a new project and choose a sample template from ASP.NET Core Web App in Visual Studio
2. Install and enable IIS server to serve the files.
3. Edit the host file in `C: > Windows > System32 > drivers > etc` and add your domain againt local ip that is 127.0.0.0
4. Go to jenkins install and configure MSbuild plugin by giving the path to `msbuild.exe` file
5. Create a jenkins pipeline job and add this
```
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
```

## Deployment command description:
```
"\"${tool 'MSBuild'}\" AspDotNetJenkins.sln----- this part of the code is running the MSBuild configured in Jenkins on the sln file.
/p:DeployOnBuild=true----- this part tells MS-Build to deploy on build
/p:DeployDefaultTarget=devops---- Use the devops to Deploy
/p:WebPublishMethod=FileSystem-------Use FileSystem for Publishing
/p:SkipInvalidConfigurations=true ----Skip Invalid Configurations
/t:build ---Do only the build
/p:Configuration=Release----Do the buld in release mode
/p:Platform=\”Any CPU\”----- For any type of CPU
/p:DeleteExistingFiles=True ----Empty the folder publishing the file
/p:publishUrl=c:\\inetpub\\wwwroot ----Publish at mentioned location
```




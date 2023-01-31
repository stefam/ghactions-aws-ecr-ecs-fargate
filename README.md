# Introduction
This project aims to describe how to create an conteneraized application using docker and deploy this app in a servless ecs cluster using aws fargate. The deployment should be automated through github actions which will build and push the images to aws ecr.

## Required
- .net 6 or greater
- dotnet cli
- docker
- vs code + docker extension
- git

## Add services and docker-compose
1. git clone {your repo url}
2. cd {your repo folder}
3. dotnet new sln
4. dotnet new mvc -o web.ui
5. dotnet sln add ./web.ui/web.ui.csproj
6. dotnet new webapi -o web.api
7. dotnet sln add ./web.api/web.api.csproj
8. code .
9. ctrl + shift + p 
   > Docker: Add Docker Files to Workspace
   > Yes (when asking to create a docker compose file)

## Add github actions file
1. mkdir .github
2. cd .github
3. mkdir workflows
4. cd workflows
5. echo > aws.yml






# Manual Push of the images to aws ecr
## Build and list images
1. docker compose build
2. docker image list







# docker build -f ./web.api/Dockerfile -t webapi:1.3 .
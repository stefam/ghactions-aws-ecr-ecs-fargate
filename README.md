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

## Configure AWS CLI access in order to create AWS Container Registry
`> aws configure` \
`AWS Access Key ID [None]: xxxxxxxxxx` \
`AWS Secret Access Key [None]: 52/SFG7mbytRbTeOsDGtypP7U4tPwlFhhMfGWan2` \
`Default region name [None]: {region}` \
`Default output format [None]: json`


## Create aws ecr
1. aws ecr create-repository --repository-name webui
2. aws ecr create-repository --repository-name webapi
3. Login docker to the AWS Container Registry: \
`aws ecr get-login-password --region {region} | docker login --username AWS --password-stdin {aws_account_id}.dkr.ecr.{region}.amazonaws.com`






# Manual Push of the images to aws ecr
## Build, list and push images to aws ecr
1. docker compose build
2. aws ecr get-login-password --region {region} | docker login --username AWS --password-stdin {aws_account_id}.dkr.ecr.{region}.amazonaws.com
3. docker images
4. docker tag webui {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webui:latest
5. docker push {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webapi:latest


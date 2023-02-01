# Introduction
This project aims to describe how to create an conteneraized application using docker and deploy this app in a servless ecs cluster using aws fargate. The deployment should be automated through github actions which will build and push the images to aws ecr.

## Required
- .NET 6 or greater
- dotnet cli
- Docker engine
- VS Code + Docker extension
- GIT

## Add services and docker-compose
1. Clone your repository to your local: \
`git clone {your repo url}`

2. Go to the app directory: \
`cd {your repo folder}`

3. Create dotnet solution: \
`dotnet new sln`

4. Create a ASP.NET MVC project: \
`dotnet new mvc -o web.ui`

5. Add the MVC project to the solution: \
`dotnet sln add ./web.ui/web.ui.csproj`

6. Create a ASP.NET Web Api project: \
`dotnet new webapi -o web.api`

7. Add the ASP.NET Web Api project to the solution: \
`dotnet sln add ./web.api/web.api.csproj`

8. Open local folder on VS Code: \
`code .`

9. Add docker files and docker compose to the project: \
> Ctrl + Shift + P \
> Docker: Add Docker Files to Workspace \
> Yes (when asking to create a docker compose file) \

## Create github actions file structure
1. `mkdir .github`

2. `cd .github`

3. `mkdir workflows`

4. `cd workflows`

5. `echo > aws.yml`

## Configure AWS CLI access before creating the AWS container repositories
`> aws configure` \
`AWS Access Key ID [None]: xxxxxxxxxxxxxxxx` \
`AWS Secret Access Key [None]: xxxxxxxxxxxxxxxx` \
`Default region name [None]: {region}` \
`Default output format [None]: json`

## Create aws ecr
1. Creates web.ui container registry: \
`aws ecr create-repository --repository-name webui`

2. Creates web.api container registry: \
`aws ecr create-repository --repository-name webapi`

3. Login docker to the aws container registry: \
`aws ecr get-login-password --region {region} | docker login --username AWS --password-stdin {aws_account_id}.dkr.ecr.{region}.amazonaws.com`

# This section shows how to build and push the images manually

## Build, Tag and Push images
We can build our images using docker compose: \
`docker compose build`

## Tag images
We can tag our images in different ways.
1. Tag images on the build: \
`docker build -f ./web.ui/Dockerfile -t {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webui:latest .` \
`docker build -f ./web.api/Dockerfile -t {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webapi:latest .`

2. Tag images based on the image name: \
`docker tag webui {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webui:latest` \
`docker tag webapi {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webapi:latest`

## Push images
`docker push {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webui:latest` \
`docker push {aws_account_id}.dkr.ecr.{region}.amazonaws.com/webapi:latest`

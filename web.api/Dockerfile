FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
#EXPOSE 5023
EXPOSE 80
EXPOSE 443

#ENV ASPNETCORE_URLS=http://+:5023
ENV ASPNETCORE_URLS=https://+:443;http://+:80

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["web.api/web.api.csproj", "web.api/"]
RUN dotnet restore "web.api/web.api.csproj"
COPY . .
WORKDIR "/src/web.api"
RUN dotnet build "web.api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "web.api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "web.api.dll"]

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

# Restore packages
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["web.api/web.api.csproj", "web.api/"]
RUN dotnet restore "web.api/web.api.csproj"

# Build the app
COPY . .
WORKDIR "/src/web.api"
RUN dotnet build "web.api.csproj" -c Release -o /app/build

# Create https certificate
RUN dotnet dev-certs https --trust

# Publish project
FROM build AS publish
RUN dotnet publish "web.api.csproj" -c Release -o /app/publish /p:UseAppHost=false

RUN dotnet dev-certs https --trust

# Run the app
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
# Configure server certificate 
#COPY ["cert.pfx", "/https/cert.pfx"]

ENTRYPOINT ["dotnet", "web.api.dll"]

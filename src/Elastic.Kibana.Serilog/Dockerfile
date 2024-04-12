# Use the official ASP.NET Core runtime image
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base

ENV ASPNETCORE_URLS=http://+:5000
WORKDIR /app

# Build stage
FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["Elastic.Kibana.Serilog.csproj", "."]
RUN dotnet restore "./Elastic.Kibana.Serilog.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "Elastic.Kibana.Serilog.csproj" -c Release -o /app/build

# Publish stage
FROM build AS publish
RUN dotnet publish "Elastic.Kibana.Serilog.csproj" -c Release -o /app/publish

# Final stage
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Elastic.Kibana.Serilog.dll"]

#docker build -t app -f Dockerfile ../Elastic.Kibana.Serilog
#docker run -d --name app --network=docker_elastic --rm -p 8000:5000 app
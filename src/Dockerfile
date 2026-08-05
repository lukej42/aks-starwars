# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /app

# Blazor WASM builds on Linux require Python + wasm-tools
RUN apt-get update \
    && apt-get install -y --no-install-recommends python3 \
    && dotnet workload install wasm-tools \
    && rm -rf /var/lib/apt/lists/*

COPY . ./
RUN dotnet restore "./Server/Star-Wars.Server.csproj"
RUN dotnet publish "./Server/Star-Wars.Server.csproj" -c Release -o /app/publish

# Stage 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:9.0
WORKDIR /app
COPY --from=build /app/publish .

ENV ASPNETCORE_URLS=http://+:80
EXPOSE 80

ENTRYPOINT ["dotnet", "Star-Wars.Server.dll"]

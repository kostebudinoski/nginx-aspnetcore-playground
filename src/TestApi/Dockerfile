FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base

WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["src/TestApi/TestApi.csproj", "src/TestApi/"]
RUN dotnet restore "src/TestApi/TestApi.csproj"
COPY . .
WORKDIR "/src/src/TestApi"
RUN dotnet build "TestApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TestApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS http://*:5000
ENV ASPNETCORE_ENVIRONMENT="production"
ENTRYPOINT ["dotnet", "TestApi.dll"]

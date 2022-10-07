#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["ApiResources/ApiResources.WebApi/ApiResources.WebApi.csproj", "ApiResources/ApiResources.WebApi/"]
COPY ["ApiResources/ApiResources.Infrastructure.Persistence/ApiResources.Infrastructure.Persistence.csproj", "ApiResources/ApiResources.Infrastructure.Persistence/"]
COPY ["ApiResources/ApiResources.Application/ApiResources.Application.csproj", "ApiResources/ApiResources.Application/"]
COPY ["ApiResources/ApiResources.Domain/ApiResources.Domain.csproj", "ApiResources/ApiResources.Domain/"]
COPY ["ApiResources/ApiResources.Infrastructure.Shared/ApiResources.Infrastructure.Shared.csproj", "ApiResources/ApiResources.Infrastructure.Shared/"]
RUN dotnet restore "ApiResources/ApiResources.WebApi/ApiResources.WebApi.csproj"
COPY . .
WORKDIR "/src/ApiResources/ApiResources.WebApi"
RUN dotnet build "ApiResources.WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ApiResources.WebApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ApiResources.WebApi.dll"]
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
RUN apt-get update && apt-get install -y libgdiplus
RUN sed -i'.bak' 's/$/ contrib/' /etc/apt/sources.list
RUN apt-get update; apt-get install -y ttf-mscorefonts-installer fontconfig

WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["AsposeWebApp/AsposeWebApp.csproj", "AsposeWebApp/"]
RUN dotnet restore "AsposeWebApp/AsposeWebApp.csproj"
COPY . .
WORKDIR "/src/AsposeWebApp"
RUN dotnet build "AsposeWebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AsposeWebApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AsposeWebApp.dll"]
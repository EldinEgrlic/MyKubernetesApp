#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["MyKubernetesApp.csproj", "MyKubernetesApp/"]
RUN dotnet restore "MyKubernetesApp/MyKubernetesApp.csproj"
WORKDIR "/src/MyKubernetesApp"
COPY . .
RUN dotnet build "MyKubernetesApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyKubernetesApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyKubernetesApp.dll"]
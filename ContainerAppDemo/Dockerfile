FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["ContainerAppDemo/ContainerAppDemo.csproj", "ContainerAppDemo/"]
RUN dotnet restore "./ContainerAppDemo/ContainerAppDemo.csproj"
COPY . .
RUN dotnet build "./ContainerAppDemo/ContainerAppDemo.csproj"  -c Release  -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "./ContainerAppDemo/ContainerAppDemo.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ContainerAppDemo.dll"]

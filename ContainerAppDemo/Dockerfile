FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["ContainerAppDemo.csproj", "."]
RUN dotnet restore "ContainerAppDemo.csproj"

COPY . .
RUN dotnet publish "ContainerAppDemo.csproj" \
    -c Release \
    -o /app/publish \
    /p:UseAppHost=false

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "ContainerAppDemo.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "ContainerAppDemo.dll"]

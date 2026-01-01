FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/3Garfos.WebApi/3Garfos.WebApi.csproj", "3Garfos.WebApi/"]
RUN dotnet restore "3Garfos.WebApi/3Garfos.WebApi.csproj"
COPY src/3Garfos.WebApi/. 3Garfos.WebApi/.
WORKDIR "/src/3Garfos.WebApi"
RUN dotnet build "./3Garfos.WebApi.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./3Garfos.WebApi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "3Garfos.WebApi.dll"]

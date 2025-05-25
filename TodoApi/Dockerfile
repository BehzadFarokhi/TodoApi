# --- BASE IMAGE (unchanged) -------------------------------------
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080 8081

# --- BUILD -------------------------------------------------------
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Project file
COPY ["TodoApi.csproj", "."]
RUN dotnet restore "TodoApi.csproj"

# Source
COPY . .
# stay in /src – there is **no /src/TodoApi** directory
RUN dotnet build "TodoApi.csproj" -c $BUILD_CONFIGURATION -o /app/build

# --- PUBLISH -----------------------------------------------------
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "TodoApi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# --- FINAL -------------------------------------------------------
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TodoApi.dll"]

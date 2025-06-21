# Dockerfile multi-stage

# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src 

# Copier le fichier .csproj et restaurer les dépendances
COPY ["Email.Api/Email.Api.csproj", "Email.Api/"]
RUN dotnet restore "Email.Api/Email.Api.csproj"

COPY . . 
WORKDIR "/src/Email.Api" 
RUN dotnet build "Email.Api.csproj" -c Release -o /app/build

# Stage 2: Publish 
FROM build AS publish
RUN dotnet publish "Email.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Stage 3: Final runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

RUN adduser -u 1997 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

ENTRYPOINT ["dotnet", "Email.Api.dll"]
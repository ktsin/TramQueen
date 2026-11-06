FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["TrainQueen.BackEnd/TrainQueen.BackEnd/TrainQueen.BackEnd.csproj", "TrainQueen.BackEnd/TrainQueen.BackEnd/"]
COPY ["TrainQueen.BackEnd/TrainQueen.BackEnd.Client/TrainQueen.BackEnd.Client.csproj", "TrainQueen.BackEnd/TrainQueen.BackEnd.Client/"]
RUN dotnet restore "TrainQueen.BackEnd/TrainQueen.BackEnd/TrainQueen.BackEnd.csproj"
COPY . .
WORKDIR "/src/TrainQueen.BackEnd/TrainQueen.BackEnd"
RUN dotnet build "./TrainQueen.BackEnd.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./TrainQueen.BackEnd.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TrainQueen.BackEnd.dll"]

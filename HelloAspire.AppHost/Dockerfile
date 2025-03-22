FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 5023

ENV ASPNETCORE_URLS=http://+:5023

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["HelloAspire.AppHost/HelloAspire.AppHost.csproj", "HelloAspire.AppHost/"]
RUN dotnet restore "HelloAspire.AppHost/HelloAspire.AppHost.csproj"
COPY . .
WORKDIR "/src/HelloAspire.AppHost"
RUN dotnet build "HelloAspire.AppHost.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "HelloAspire.AppHost.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HelloAspire.AppHost.dll"]

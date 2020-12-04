FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build-env

WORKDIR /app

COPY ["HealthCheckExampleApp.csproj", ""]
RUN dotnet restore "./HealthCheckExampleApp.csproj"

COPY . .

RUN dotnet publish "HealthCheckExampleApp.csproj"  -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim

WORKDIR /app
COPY --from=build-env /app/out .

EXPOSE 80
EXPOSE 443

RUN groupadd -g 500 dotnet && \
   useradd -u 500 -g dotnet dotnet \
   && chown -R dotnet:dotnet /app

USER dotnet:dotnet

ENV ASPNETCORE_URLS http://*:5000

ENTRYPOINT ["dotnet", "HealthCheckExampleApp.dll"]
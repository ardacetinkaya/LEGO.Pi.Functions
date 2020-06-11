FROM mcr.microsoft.com/azure-functions/dotnet:3.0-arm32v7 AS base
WORKDIR /home/site/wwwroot
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["LEGO.Pi.Functions.csproj", "LEGO.Pi.Functions/"]
RUN dotnet restore "LEGO.Pi.Functions/LEGO.Pi.Functions.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "LEGO.Pi.Functions.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "LEGO.Pi.Functions.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /home/site/wwwroot
COPY --from=publish /app/publish .
RUN ls
ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
    AzureFunctionsJobHost__Logging__Console__IsEnabled=true



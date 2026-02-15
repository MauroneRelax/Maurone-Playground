FROM mcr.microsoft.com/dotnet/runtime:10.0 AS base
WORKDIR /app

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["CSharpBase/CSharpBase.csproj", "CSharpBase/"]
RUN dotnet restore "CSharpBase/CSharpBase.csproj"
COPY . .
WORKDIR "/src/CSharpBase"
RUN dotnet build "CSharpBase.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "CSharpBase.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CSharpBase.dll"]

FROM --platform=$TARGETPLATFORM mcr.microsoft.com/dotnet/aspnet:7.0-bookworm-slim AS base
WORKDIR /app
EXPOSE 80

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:7.0-bookworm-slim AS build
ARG TARGETOS
ARG TARGETARCH

WORKDIR /src
COPY "TestWebApplication/" "TestWebApplication/"
RUN dotnet restore "TestWebApplication/TestWebApplication.csproj" -a $TARGETARCH

FROM build AS publish
WORKDIR "/src/TestWebApplication"
RUN dotnet publish "TestWebApplication.csproj" -c Release --os $TARGETOS -a $TARGETARCH --no-restore -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TestWebApplication.dll"]
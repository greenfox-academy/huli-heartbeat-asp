FROM microsoft/dotnet:2.0-runtime AS base
WORKDIR /app

FROM microsoft/dotnet:2.0-sdk AS build
WORKDIR /src
COPY *.sln ./
COPY HeartBeatTest/HeartBeatTest.csproj HeartBeatTest/
COPY HeartBeat/HeartBeat.csproj HeartBeat/
RUN dotnet restore
COPY . .
WORKDIR /src/HeartBeatTest
RUN dotnet build -c Release -o /app
EXPOSE 80/tcp
RUN chmod +x ./entrypoint.sh
CMD /bin/bash ./entrypoint.sh

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "HeartBeatTest.dll"]

ARG NODE_IMAGE=node:8.11
FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.1-sdk as dotnet-build
WORKDIR /src

FROM ${NODE_IMAGE} as node-build
WORKDIR /web
COPY src/Web/SPA/Rpi.SPA.Web .
RUN npm install --prefix ./ClientApp
RUN npm run build --prefix ./ClientApp

FROM dotnet-build as publish
WORKDIR /src/src/Web/SPA/Rpi.SPA.Web/ClientApp/build
COPY --from=node-build /web/ClientApp/build .
WORKDIR /src
COPY . .
WORKDIR /src/src/Web/SPA/Rpi.SPA.Web
RUN dotnet publish -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Rpi.SPA.Web.dll"]
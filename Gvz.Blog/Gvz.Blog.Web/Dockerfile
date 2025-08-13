FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Gvz.Blog.Web/Gvz.Blog.Web.csproj", "Gvz.Blog.Web/"]
RUN dotnet restore "Gvz.Blog.Web/Gvz.Blog.Web.csproj"
COPY . .
WORKDIR "/src/Gvz.Blog.Web"
RUN dotnet build "./Gvz.Blog.Web.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Gvz.Blog.Web.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Gvz.Blog.Web.dll"]

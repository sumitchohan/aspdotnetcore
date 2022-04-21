#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-container
WORKDIR /src
 

WORKDIR "/src/."
COPY ["dotnetcore/dotnetcore.csproj", "dotnetcore/"]
RUN dotnet restore "dotnetcore/dotnetcore.csproj"
COPY . .

RUN dotnet build "dotnetcore/dotnetcore.csproj" -c Release -o /app/build

#FROM build AS publish
RUN dotnet publish "dotnetcore/dotnetcore.csproj" -c Release -o /app/publish
EXPOSE 3000
ENV ASPNETCORE_URLS=http://+:3000

FROM mcr.microsoft.com/dotnet/aspnet:6.0

RUN adduser --disabled-password \
  --home /app \
  --gecos '' dotnetuser && chown -R dotnetuser /app

# impersonate into the new user
USER dotnetuser

WORKDIR /app 
COPY --from=build-container /app/publish .
EXPOSE 3000
ENV ASPNETCORE_URLS=http://+:3000 
CMD ["dotnet", "dotnetcore.dll"]

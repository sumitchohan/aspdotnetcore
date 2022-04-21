#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-container
WORKDIR /src

WORKDIR "/src/."
COPY ["MvcMovie/MvcMovie.csproj", "MvcMovie/"]
RUN dotnet restore "MvcMovie/MvcMovie.csproj"
COPY . .

RUN dotnet build "MvcMovie/MvcMovie.csproj" -c Release -o /app/build

#FROM build AS publish
RUN dotnet publish "MvcMovie/MvcMovie.csproj" -c Release -o /app/publish
EXPOSE 3000
ENV ASPNETCORE_URLS=http://+:3000

FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build-container /app/publish .
EXPOSE 3000
ENV ASPNETCORE_URLS=http://+:3000
CMD ["dotnet", "MvcMovie.dll"]

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-nanoserver-1903 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-nanoserver-1903 AS build
WORKDIR /src
COPY ["main/Port.Adapter/UI/Views/Blazor/Blazor.csproj", "main/Port.Adapter/UI/Views/Blazor/"]
COPY ["main/Port.Adapter/UI/Common/Common.csproj", "main/Port.Adapter/UI/Common/"]
COPY ["main/Domain.Model/Domain.Model.csproj", "main/Domain.Model/"]
COPY ["main/Port.Adapter/IO/Process/Services/Services.csproj", "main/Port.Adapter/IO/Process/Services/"]
COPY ["main/Application/Application.csproj", "main/Application/"]
RUN dotnet restore "main/Port.Adapter/UI/Views/Blazor/Blazor.csproj"
COPY . .
WORKDIR "/src/main/Port.Adapter/UI/Views/Blazor"
RUN dotnet build "Blazor.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Blazor.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "d23-blazor.dll"]
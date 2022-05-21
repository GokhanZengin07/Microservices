#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app


FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Services/ReservationAPI/ReservationAPI/ReservationAPI.csproj", "Services/ReservationAPI/ReservationAPI/"]
COPY ["Services/ReservationAPI/Reservation.API.Services/Reservation.API.Services.csproj", "Services/ReservationAPI/Reservation.API.Services/"]
COPY ["Services/ReservationAPI/Reservation.API.Infrastructure/Reservation.API.Infrastructure.csproj", "Services/ReservationAPI/Reservation.API.Infrastructure/"]
COPY ["Reservation.API.Models/Reservation.API.Models.csproj", "Reservation.API.Models/"]
RUN dotnet restore "Services/ReservationAPI/ReservationAPI/ReservationAPI.csproj"
COPY . .
WORKDIR "/src/Services/ReservationAPI/ReservationAPI"
RUN dotnet build "ReservationAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ReservationAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ReservationAPI.dll"]
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
# FROM mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim-arm32v7 AS base
FROM mcr.microsoft.com/dotnet/aspnet:3.1-buster-slim-arm64v8 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
# FROM mcr.microsoft.com/dotnet/sdk:3.1.409-bionic-arm32v7 AS build
# FROM mcr.microsoft.com/dotnet/sdk:3.1-bionic-arm32v7 AS build
# FROM mcr.microsoft.com/dotnet/sdk:3.1.409-focal-arm32v7 AS build
# FROM mcr.microsoft.com/dotnet/sdk:3.1.409-buster-arm32v7 AS build
# FROM mcr.microsoft.com/dotnet/sdk:3.1-buster-arm32v7 AS build
# FROM mcr.microsoft.com/dotnet/sdk:5.0.300-focal-arm32v7 AS build
# FROM mcr.microsoft.com/dotnet/sdk:3.1.409-buster-arm64v8 AS build
FROM mcr.microsoft.com/dotnet/sdk:3.1-buster-arm64v8 AS build
WORKDIR /src
COPY ["studyDockerArm/studyDockerArm.csproj", "studyDockerArm/"]
# COPY ["studyDockerArm.csproj", "studyDockerArm/"]
# RUN dotnet restore "studyDockerArm/studyDockerArm.csproj" -r linux-arm
RUN dotnet restore "studyDockerArm/studyDockerArm.csproj" -r linux-arm64
COPY . .
WORKDIR /src/studyDockerArm
RUN dotnet build "studyDockerArm.csproj" -r linux-arm64 -c Release -o /app/build 

FROM build AS publish
RUN dotnet publish "studyDockerArm.csproj" -r linux-arm64 -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "studyDockerArm.dll"]



# ------
#  > [4/5] RUN dotnet restore "studyDockerArm/studyDockerArm.csproj":
# #8 0.268 A fatal error occurred, the folder [/usr/share/dotnet/host/fxr] does not contain any version-numbered child folders
# ------
# Dockerfile:15
# --------------------
#   13 |     # COPY ["studyDockerArm/studyDockerArm.csproj", "studyDockerArm/"]
#   14 |     COPY ["studyDockerArm.csproj", "studyDockerArm/"]
#   15 | >>> RUN dotnet restore "studyDockerArm/studyDockerArm.csproj"
#   16 |     COPY . .
#   17 |     # WORKDIR /src/studyDockerArm
# --------------------
# error: failed to solve: rpc error: code = Unknown desc = executor failed running [/bin/sh -c dotnet restore "studyDockerArm/studyDockerArm.csproj"]: exit code: 131

# ↑これは久しぶりにやったら成功した。あとパスの問題もあったので、ビルドオプションに-fをつけた
# docker buildx build -f studyDockerArm/Dockerfile --platform linux/arm/v7 -t moss7/studydockerarm3 --push .




# docker buildx build --platform linux/arm64 -t moss7/studydockerarm3 --push .<- kore
# docker buildx build --platform linux/arm/v7 -t moss7/studydockerarm3 --push . 


# Failed to load t error: /app/libhostpolicy.so: wrong ELF class: ELFCLASS64
# An error occurred while loading required library libhostpolicy.so from [/app/]

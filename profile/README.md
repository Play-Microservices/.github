# Play.Microservices

Welcome to my training project with Microservice architecture!
Inside you can find all repositories required and used to set up, build and run own solution based on microservice architecture

## Tech stack

In order to run this application you need to install on your machine
- Any OS platform thanks to .NET Core cross-platform architecture
- .NET SDK version 8
- Docker with the following applications:
    - Mongo DB
    - RabbitMQ

## Development

For development purposes I recommend using the following tools:
- .NET SDK 8
- Docker
- Git with Git Bash
- Visual Studio Code (you can use Visual Studio but I do not recommend it for such small services like these)

## Objective

The application is service APIs with game features allowing users to create items and then assign items to their inventory.
This can be achieved using API or Front-End Web application with simple GUI.
Application has to provide high standard quality, be fast with customer responses and be able to scale up and down based on customer's requests.

At this moment application does not have implemented solution for scaling, but everything else is implemented in provided repositories.

## Repositories

Based on microservices architecture, I decided to split services into autonomous entities that have separate codebase that works without having direct links to other services. I will explain goal and purpose of each of them.

### Play.Catalog

This repository contains two projects:

API providing API with functionality related to item definition in the game.
User can create item based on which later users or other services can use and refer in the game.

Library with contract definition for catalog items changes.
This contract is used for publish/subscribe pattern and used by other services and Message Broker

### Play.Inventory

API providing API with functionality related to user inventory.
Thank to this users can assign/remove/update any items available in the system (exactly in Player.Catalog repository).
It works in asynchronous way with Play.Catalog service.

### Play.Common

Library with common code used across other repositories.
It provides implementation and set of helpful methods to simplify usage of some patterns used across whole solution.
For example it is: Repository, Configuration settings, Broken Circuit etc.

### Play.Infrastructure

Repository providing all tools required to set up application and run it in the simple repetitive way.
At this moment it contains docker-compose.yml file with configuration of all necessary images used by services (for development purposes atm).

In future this repository will be responsible for CI/CD staff and things related with cloud provider (in my case it will be Azure).

## Deployment

1. Download all repositories into single folder - for example *Play.Microservices*.
2. Navigate to Play.Common folder with source files and type in console `dotnet build` to build project.
3. Then type `dotnet pack -o ../../../packages/ -p PackageVersion=1.0.4` to publish nuget package.
4. Navigate to Play.Catalog.Contracts folder with source file and type in console `dotnet build` to build project.
5. Then type `dotnet pack -o ../../../packages/ -p PackageVersion=1.0.1` to publish nuget package.
6. Now you need to start MongoDB and RabbitMQ by goint to Play.Infrastructure and running command `docker compose up`.
7. Go to Play.Catalog.API and type `dotnet build`.
8. Start Catalog API in new console `dotnet run`.
9. Go to Play.Inventory.API and type `dotnet build`.
10. Start Inventory API in new console `dotnet run`.
11. Go to Play.Frontend and type `npm build`.
12. Start frontend server in new console `npm start`.

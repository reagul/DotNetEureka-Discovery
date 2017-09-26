# DotNetEureka-Discovery Sample - Precompiled

# Setup Eureka service on CloudFoundry
You must first create an instance of the Circuit Breaker service in a org/space.

1. cf target -o myorg -s development
2. cf create-service p-service-registry standard myDiscoveryService
3. Wait for the service to become ready! (i.e. Type `cf services` and make sure the services are showing up as created ) 

# Publish App & Push to CloudFoundry

1. cf target -o myorg -s development
2. cd to the respective Publish folder 
3. Optional if you have Source: Publish app to a directory selecting the framework and runtime you want to run on. 
(`dotnet publish  -f netcoreapp2.0 -r ubuntu.14.04-x64`)
6. Push the app using the appropriate manifest.
 (`cf push -f manifest.yml -p PUBLISH-FT-Service` or `cf push -f manifest-windows.yml -p PUBLISH-FT-Service`)

Note: If you are using self-signed certificates it is possible that you might run into SSL certificate validation issues when pushing this app. The simplest way to fix this:

1. Optional: This should already be in place in the appsettings.json file in the respective Publish folders. 
Disable certificate validation for the Spring Cloud Discovery Client.  You can do this by editing `appsettings.json` and add `eureka:client:"validate_certificates:" false`.

# DotNetEureka-Discovery Sample - Precompiled

# Setup Circuit Breaker Dashboard service on CloudFoundry
You must first create an instance of the Circuit Breaker service in a org/space.

1. cf target -o myorg -s development
2. cf create-service p-circuit-breaker-dashboard standard myHystrixService
3. Wait for the service to become ready! (i.e. Type `cf services` and make sure the services are showing up as created ) 

# Publish App & Push to CloudFoundry

1. cf target -o myorg -s development
2. cd samples/CircuitBreaker/src/AspDotNetCore/Fortune-Teller/Fortune-Teller-UI
3. Make sure environment variable `BUILD` is not set to `LOCAL` (i.e. SET BUILD=, unset BUILD)
4. dotnet restore --configfile nuget.config
5. Only if you have Source Code : Publish app to a directory selecting the framework and runtime you want to run on. 
(`dotnet publish  -f netcoreapp2.0 -r ubuntu.14.04-x64`)
6. Push the app using the appropriate manifest.
 (`cf push -f manifest.yml -p PUBLISH-FT-Service` or `cf push -f manifest-windows.yml -p PUBLISH-FT-Service`)


Note: If you are using self-signed certificates it is possible that you might run into SSL certificate validation issues when pushing this app. The simplest way to fix this:

1. Optional: This should already be in place in the appsettings.json file in the respective Publish folders. 
Disable certificate validation for the Spring Cloud Discovery Client.  You can do this by editing `appsettings.json` and add `eureka:client:validate_certificates=false`.

# What to expect - CloudFoundry
After building and running the app, you should see something like the following in the logs. 

To see the logs as you startup and use the app: `cf logs fortuneui`

On a Windows cell, you should see something like this during startup:
```
2016-05-14T06:38:21.67-0600 [CELL/0]     OUT Successfully created container
2016-05-14T06:38:47.90-0600 [APP/0]      OUT dbug: Steeltoe.Discovery.Eureka.Transport.EurekaHttpClient[0]
2016-05-14T06:38:47.90-0600 [APP/0]      OUT       DoGetApplicationsAsync .....
2016-05-14T06:38:47.91-0600 [APP/0]      OUT dbug: Steeltoe.Discovery.Eureka.DiscoveryClient[0]
2016-05-14T06:38:47.91-0600 [APP/0]      OUT       FetchFullRegistry returned: OK, Applications[Application[Name=FORTUNESERVICE ....
2016-05-14T06:38:47.91-0600 [APP/0]      OUT dbug: Steeltoe.Discovery.Eureka.DiscoveryClient[0]
2016-05-14T06:38:47.91-0600 [APP/0]      OUT       FetchRegistry succeeded
2016-05-14T06:38:47.99-0600 [APP/0]      OUT verb: Microsoft.AspNet.Hosting.Internal.HostingEngine[4]
2016-05-14T06:38:47.99-0600 [APP/0]      OUT       Hosting starting
2016-05-14T06:38:48.02-0600 [APP/0]      OUT info: Microsoft.Net.Http.Server.WebListener[0]
2016-05-14T06:38:48.02-0600 [APP/0]      OUT       Start
2016-05-14T06:38:48.07-0600 [APP/0]      OUT info: Microsoft.Net.Http.Server.WebListener[0]
2016-05-14T06:38:48.07-0600 [APP/0]      OUT       Listening on prefix: http://*:58442/
2016-05-14T06:38:48.12-0600 [APP/0]      OUT       Hosting started
2016-05-14T06:38:48.12-0600 [APP/0]      OUT verb: Microsoft.AspNet.Hosting.Internal.HostingEngine[5]
2016-05-14T06:38:48.12-0600 [APP/0]      OUT Hosting environment: development
2016-05-14T06:38:48.12-0600 [APP/0]      OUT Now listening on: http://*:58442
2016-05-14T06:38:48.12-0600 [APP/0]      OUT Application started. Press Ctrl+C to shut down.
```
At this point the Fortune Teller UI is up and running and ready for displaying your fortune. Hit http://fortuneui.x.y.z/ to see it!

In addition to hitting http://fortuneui.x.y.z/, you can also hit: http://fortuneui.x.y.z/#/multiple to cause the UI to make use of a Hystrix Collapser to obtain multiple fortunes.

# Using the Hystrix Dashboard - Cloud Foundry

Once you have the two applications communicating, you can make use of the Hystrix dashboard by following the instructions below.  

1. Open a browser or browser window and connect to the Pivotal Apps Manager.  You will have to use a link that is specific to your Cloud Foundry setup. (e.g. https://apps.system.testcloud.com)
2. Follow [these instructions](http://docs.pivotal.io/spring-cloud-services/1-3/common/circuit-breaker/using-the-dashboard.html) to open the Hystrix dashboard service.
3. Go back to the Fortune-Teller-UI application and obtain several fortunes.  Observe the values changing in the Hystrix dashboard.  Click the refresh button on the UI app quickly to see the dashboard update.

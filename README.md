## Grails plugin for integrating the Atmosphere Framework
https://github.com/Atmosphere/atmosphere/wiki

The plugin uses the following pieces of the Atmosphere Framework:

* jquery.atmosphere.js (https://github.com/Atmosphere/atmosphere/wiki/jQuery.atmosphere.js-API)

* MeteorServlet (http://atmosphere.github.com/atmosphere/apidocs/org/atmosphere/cpr/MeteorServlet.html)


* ReflectorServletProcessor (http://atmosphere.github.com/atmosphere/apidocs/org/atmosphere/handler/ReflectorServletProcessor.html)

* DefaultBroadcaster (http://atmosphere.github.com/atmosphere/apidocs/org/atmosphere/cpr/DefaultBroadcaster.html)

* SimpleBroadcaster (http://atmosphere.github.com/atmosphere/apidocs/org/atmosphere/util/SimpleBroadcaster.html)

## How It Works

### Java Servlet

The plugin is designed to create and use a servlet for each main or significant URL pattern. For example, if you install the plugin as a standalone application, you will see that a servlet is created for each URL pattern below:

	/jabber/chat/*

	/jabber/notification/*

	/jabber/public/*

The servlets are created programmatically using ServletContext.addServlet and are not defined in web.xml.

### Configuration

The configuration file, grails-app/conf/Atmosphere2Config.groovy, is used to tie the MeteorServlet and MeteorHandler classes together.

### MeteorServlet Class

The create-meteor-servlet script creates a class in grails-app/atmosphere that extends Atmosphere's MeteorServlet. You could probably use a single class throughout your application.

Although the example application uses the same MeteorServlet class for each URL, you can easily use a different class. Of course, each of the URL patterns above can be further divided using a combination of request headers, Broadcaster, etc. For example, a chat room could be established under /jabber/chat/private-room/* that is serviced by the same servlet, MeteorServlet, and MeteorHandler classes as /jabber/chat/*.

### MeteorHandler Class

The create-meteor-handler script creates a class in grails-app/atmosphere that extends HttpServlet. This is where you customize how the incoming and outgoing (including Atmosphere Broadcaster) HTTP requests are handled.

## Standalone Application Installation

The plugin source can be downloaded and used as a standalone Grails application. I suggest running it first before installing the plugin.

 1. Clone the plugin repository

 2. cd /path/to/grails-atmosphere2

 3. grails run-app

You will have a simple application that performs the following tasks out of the box. Please note that this sample is not production ready. It merely incorporates some of the lessons I have learned and provides a point of departure for your own application.

* Chat (open two different browsers on your computer and start chatting)

* Sends a one-time, client-triggered notification to subscribers

* Automatically updates the web page at predefined intervals

You can review the files below to understand how it all works. Note that many of the files are not packaged into the finished plugin.

* grails-app/atmosphere/org/grails/plugins/atmosphere2/DefaultMeteorHandler.groovy

* grails-app/atmosphere/org/grails/plugins/atmosphere2/DefaultMeteorServlet.groovy

* grails-app/conf/Atmosphere2Config.groovy

* grails-app/controllers/org/grails/plugins/atmosphere2/AtmosphereTestController.groovy

* grails-app/services/org/grails/plugins/atmosphere2/AtmosphereTestService.groovy

* grails-app/views/AtmosphereTest/index.gsp: This file contains all internal JavaScript.

* src/groovy/org/grails/plugins/atmosphere2/ApplicationContextHolder (Burt Beckwith)

## Plugin Installation

The instructions assume you are using Tomcat as the servlet container. Since the plugin is not yet in the Grails plugin repository, the steps below are cumbersome.

1. Clone the plugin repository

2. cd /path/to/grails-atmosphere2

3. grails package-plugin

4. cd /path/to/your/application

5. grails install-plugin /path/to/grails-atmosphere2/grails-atmosphere2-x.x.x.zip

6. Create a MeteorServlet. Changes to these classes are reloaded automatically.
```groovy
    grails create-meteor-servlet com.example.Default
```

7. Create a handler. Changes to these classes are reloaded automatically.
```groovy
    grails create-meteor-handler com.example.Default
```

8. Edit grails-app/conf/Atmosphere2Config.groovy. Changes to this file will be implemented when the application is restarted.
```groovy
    import com.example.DefaultMeteorHandler

    defaultMapping = "/jabber/*"

    servlets = [
        MeteorServlet: [
        description: "MeteorServlet Default",
        className: "com.example.DefaultMeteorServlet",
        mapping: "/jabber*/",
        handler: DefaultMeteorHandler
        ]
    ]
```

9. Note the changes the plugin installation made to grails-app/conf/BuildConfig.groovy
```groovy
    grails.servlet.version = "3.0"
    grails.tomcat.nio = true

    grails.project.dependency.resolution = {
        dependencies {
            compile('org.atmosphere:atmosphere-runtime:1.0.9') {
                excludes 'slf4j-api', 'atmosphere-ping'
            }
        }
    }
```

10. Use the JavaScript code in grails-app/views/atmosphereTest/index.gsp to get you started with your own client implementation.

## Comments

Your comments, questions, and suggestions are very welcome!


# Grakka #

Grakka is a simple demonstration application showing how to integrate Grails 3.x and Akka 2.4.

Grakka integrates Grails and Akka following the same Spring context integration approach demonstrated 
in Typesafe's [activator-akka-java-spring demo](https://github.com/typesafehub/activator-akka-java-spring).

The key of the design is that the Spring context is responsible for creating both Grails objects and Akka actors.  Since Spring is responsible for object creation a number of nice features are avilable automatically:

* Grails objects may be injected with Akka objects, e.g. [SearchController](grails-app/controllers/grakka/SearchController.groovy)
* Akka actors may be injected with Grails objects, e.g. [SearchEngineActor](src/main/groovy/grakka/search/SearchEngineActor.groovy) 

The availability of bi-directional injection of resources enables the application designer to decide which way(s) dependencies should flow through the application.

## Integration Implementation ##

There are few classes that collaborate to implement the integration, all in grails-app/utils/conf:
  
* [AkkaSpringConfiguration](grails-app/utils/conf/AkkaSpringConfiguration.groovy) - a Spring context definition that defines the Akka actor system as a Spring bean, supplementing the standard Spring context defined by Grails by importing the context in [resources.groovy](grails-app/conf/spring/resources.groovy)
* [SpringExtension](grails-app/utils/conf/SpringExtension.groovy) - an extension to the Akka actor system that enables actor creation to be performed by the SpringActorProducer 
* [SpringActorProducer](grails-app/utils/conf/SpringActorProducer.groovy) - an implementation of Akka's [IndirectActorProducer](http://doc.akka.io/api/akka/2.4.0/index.html#akka.actor.IndirectActorProducer) that delegates actor creation to Spring 
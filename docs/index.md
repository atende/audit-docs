# About

The goal of this project is to create a distributed, scalable, secure and robust audit system.

1. Distribute - That means parts of the system can be deployed separed, and many applications can be monitored
2. Scalable - The system can be scaled for great performance and huge amount's of data
3. Secured - The system need to be secure
4. Robust - Being robust can mean many things: resilent, easy to maintain, fault tolerant

The project is consisted of some developed components:

* [audit-mq-collector](https://github.com/crcpuc/audit-mq-collector)
* [audit-view](https://github.com/crcpuc/audit-view)
* [audit-model](https://github.com/crcpuc/audit-model)
* [audit-appclient-spring](https://github.com/crcpuc/audit-appclient-spring)

And some infraestruture components

* [RabbitMQ](https://www.rabbitmq.com/)
* [Postgresql](www.postgresql.org/)

Every component, including the infrastructure components, could be changed. For instance you can use a cassandra database, a mongodb database, or any other database you want.


Any application could be plugged to the system, in any language, using any approach to generate the events.
That is part of the robust goal. To start we are working on spring projects with the infrastructure below. Again, should not be difficult
to provide support for other applications.

# Architecture

Each component play a role in the architecture represented by Figure 1:

![components]

Let's start by talking about what an application need to do to be audited.

A application need's to do only one thing: **Publish AuditEvents**

Suppose you want to audit every login and logout to a system. The login event is the event you need to catch in your application and publish in the message bus

The message you publish is a standard message modeled by the **AuditEvent** class in the **audit-model** component. The audit-model component just
creates a standard data transfer object class to be published, consumed and stored by the system. The message need to be in JSON format, you can serialize the AuditEvent class, but
if you use the audit-appclient-framework, it abstract that for you and try to provide a API for communication that is aware of you framework. Current, only spring
framework is supported.

But you don't need to use *audit-model* or *audit-appclient-whatever*, in your application, you just need to publish a standard json object to the bus

What is the bus? The RabbitMQ component is a robust and easy to use message queue, that is the bus I'm talking about. Current is the only supported message Queue, but probally you don't need other.

Ok. Get a standard message in JSON that represents an audit event, and publish on the message queue, easy right? What happens next?

The next component takes the action now. The **audit-mq-collector** is a message queue collector that only knows how to get a message in the queue and store it :-)

That is it, audit-mq-collector takes a message in the queue and store in the database. Current it only talks with a postgresql database, and stores in a single table for good performance

If the system grow's, you could need a cluster of postgresql databases, or you could create a cluster of the nosql of your choise for insane performance
or you could even add a new audit-mq-collector that talks to another database and replicates to a master view. 
In that case, you need to change only this component, and the audit-view

What is the audit-view?

Is just a nice look view of the database, where people can search for events. You can do that with the standard tools of the database. The advantages 
of the audit-view are:

1. Better for non tecnical users
2. Authenticates with JBoss Keycloak Server
3. Is a simple web app

Now that you know the basics, lets talk about some challends and fun:

1. Do everything async: The queue is not only to publish the events, is to make the system performant and scalable. For that goal every application that publish the events, need to do that 
in a async way.
2. Use Aspect Oriented Programming: To catch the events, aspect oriented programming is the best way. The audit-appclient-spring component will do that. We have some challends here,
sometimes it will need developer logic or annotations to represent the AuditEvent better. I'm investigating on this, check the audit-appclient-spring for more details

## Deployment

It could be implanted like in the Figure 2:

![deployment]




[deployment]: images/deployment_diagram.png "Deployment Diagram"
[components]: images/component_diagram.png "Component Diagram"
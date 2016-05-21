
<section class="page-header">
      <h1 class="project-name">Audit-docs</h1>
      <h2 class="project-tagline">Documentation for the project Audition</h2>
      <a href="https://github.com/crcpuc/audit-docs" class="btn btn-primary">View on GitHub</a>
      <a href="https://github.com/crcpuc/audit-docs/zipball/master" class="btn btn-primary">Download .zip</a>
      <a href="https://github.com/crcpuc/audit-docs/tarball/master" class="btn btn-primary">Download .tar.gz</a>
</section>

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


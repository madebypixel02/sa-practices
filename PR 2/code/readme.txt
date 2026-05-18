PRAC2 - Software Architecture
Alejandro Perez


Infrastructure

No changes were made to docker-compose.yml. To start the infrastructure:

- cd into the spring-2026/ directory
- run "docker compose up -d"
- wait until all containers are healthy (postgres databses, kafka, adminer)

You can check with "docker compose ps". The expected containers are:
- postgres_productdb on port 54320
- postgres_userdb on port 54321
- postgres_coursedb on port 54322
- postgres_credentialdb on port 54323
- kafka on port 19092
- adminer on port 18080


Microservices

The ProductCatalog (port 18081) and User (port 18082) services need to be
started first because Course validates instructor/student emails against
the User service. Their JARs are in the original repositories.

Once those are running, start the three implemented services in any order:
- java -jar course-0.0.1-SNAPSHOT.jar (port 18084)
- java -jar microcredential-0.0.1-SNAPSHOT.jar (port 18085)
- java -jar notification-0.0.1-SNAPSHOT.jar (port 18083)

All three databases use spring.jpa.hibernate.ddl-auto=create-drop so the
schema is recreated on every restart and seed data is loaded from each
project's schema.sql automatically.

No aditional configuration is needed. All inter-service URLs and ports
are already set in each service's application.properties.

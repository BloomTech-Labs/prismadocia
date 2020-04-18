# Prismatopia

[![Maintainability](https://api.codeclimate.com/v1/badges/015ff2fee461e3bc2b2b/maintainability)](https://codeclimate.com/github/Lambda-School-Labs/prismatopia/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/015ff2fee461e3bc2b2b/test_coverage)](https://codeclimate.com/github/Lambda-School-Labs/prismatopia/test_coverage)
![CI](https://github.com/Lambda-School-Labs/prismatopia/workflows/CI/badge.svg)
[![Dependency Status][daviddm-image]][daviddm-url]

## What is Prismatopia?

Prismatopia is a GraphQL API stack combining a bunch of super-awesome technologies: Apollo Server 2, Prisma, Postgres, Docker, AWS, OAuth, Make, Yeoman and more!

## The Stack

Here are the technologies in the Prismatopia stack...

### Apollo Server 2

- Provides a GraphQL server for resolvers, which is where your business logic lives

### Prisma

- Provides an ORM to translate from Graphql to Postgres, Apollo resolvers mainly call a Prisma Client to access data

### Postgres

- Provides persistent storage for data, this is managed by AWS RDS in production but is run in a container during local development

### AWS

- Handles networking (ALB, VPC, etc.) and container management (ECS)

### OAuth

- Apollo is setup for validating JWTs from clients (Works with [Okta](https://www.okta.com/) out of the box)

### Docker

- There's a local Docker Compose setup for easy development. Also, all AWS services (except Postgres) run in containers

## Useful Makefile Targets

First, it is very important to note that there is a Makefile in the root directly that is intended to provide all of the controls that you'll need for both local development and AWS operations.

### `make init`

This command will do some cleanup and will try to ensure that all required packages are in place. It's a good command to start with.

### `make docker-clean`

This command will do some cleanup of your Docker environment, which can get messy and cluttered at times.

### `make local-up`

This is a big one! It will use Docker Compose to bring up a local environment with a running Apollo Server, Prisma Server and Postgres Server.

## FAQ

??? question "Why Apollo? Why not just let clients talk to Prisma directly?"
    Because Prisma has no mechanism for executing your business logic. It exposes every possible CRUD operation to your entire data model without any controls. Apollo allows us to create resolvers so we can write business logic to carefully control CRUD against the Prisma layer. In fact, Apollo will only expose a very small subset of the API that Prisma exposes.

??? question "Why Prisma? Why not just use Apollo to talk to the database?"
    Because Prisma is a GraphQL based abstraction layer between the Apollo world of GraphQL and the database, which is Postgres in this case, but could also be many other data stores. Using Prisma allows us to work with a single data model from the frontend client to the backend business logic. This makes the whole system very flexible and fast to work with... at least that's what the brochure claims.

MIT © [Lambda School](https://lambdaschool.com)

[npm-image]: https://badge.fury.io/js/%40lambdaschool%2Fgenerator-prismatopia.svg
[npm-url]: https://www.npmjs.com/package/@lambdaschool/generator-prismatopia
[daviddm-image]: https://david-dm.org/Lambda-School-Labs/generator-prismatopia.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/Lambda-School-Labs/generator-prismatopia

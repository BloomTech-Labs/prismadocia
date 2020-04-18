# The AWS Layer

"Code all the things!" 👈

## Deploy to AWS

Now the fun part! You have your local Prisma datamodel ready and your Apollo schema and resolvers are working, time to deploy to production!

## Build the AWS infrastructure

Prismatopia uses AWS Cloudformation to build a network and all supporting infrastructure for running your GraphQL backend in AWS. The entire process is scripted using CloudFormation, which makes it repeatable and robust.

The scripts are in the `aws` folder and are divided up in order to minimize bundling too much infrastructure in a single stack.

1. Run `make aws-deploy-app` to deploy all of the application level infrastructure that's shared between different environments.
1. Run `make aws-deploy-env` to deploy all of the environment level infrastructure, including the Postgres database, Prisma service and Apollo service.
1. Now, deploy your Prisma datamodel using `make aws-prisma-deploy`
1. You can now access your services using `apollo.<yourdomain>` and `prisma.<yourdomain>`

## AWS Makefile Targets

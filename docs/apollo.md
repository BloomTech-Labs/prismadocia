# The Apollo Layer

Once you have your Prisma datamodel setup, you'll need to start working on Apollo. Remember, your clients will _only_ ever connect to Apollo, never to Prisma. Prisma is far too wide-open to expose to external clients (web apps, mobile apps, etc).

## Setting up Okta

Apollo has built-in support for authentication using an OIDC compliant identity provider. This example will focus on using Okta, but Auth0 has also been proven to work.

1. Create a new Okta developer account at <https://developer.okta.com>
1. Note your Okta domain, it will be something like: `https://dev-123456-admin.okta.com/`
1. Set the values for the following environment variables in your `.env` file:
      - `APOLLO_TOKEN_ENDPOINT=https://<your okta domain>/oauth2/default/v1/token`
      - `APOLLO_JWKS_URI=https://<your okta domain>/oauth2/default/v1/keys`
      - `APOLLO_JWT_ISSUER=https://<your okta domain>/oauth2/default`
1. Add a new Native application
    - Enable Authorization Code and Resource Owner Password grant types
    - Set the following environment variabels in your `.env` file:
        - `APOLLO_CLIENT_ID=<The Client ID for your application>`
        - `APOLLO_CLIENT_SECRET=<The Client Secret for your application>`
1. Create a test user and save the username and password in your `.env` file:
    - APOLLO_TEST_USERNAME
    - APOLLO_TEST_PASSWORD
1. At this point your should be able to successfully run `make apollo-token`
    - You can use this token in the Apollo playground: <http://localhost:8000>

## Cherry picking from the Prisma API

Your Apollo API (the one your clients _will_ talk to) will end up being be a small subset of your Prisma API. You'll only expose the parts of the Prisma API that your clients need and will leave the rest hidden to the world. This will keep your Apollo API development (and testing) to a minimum.

1. Get Prismatopia running: `make local-up`
1. Run `make prisma-generate` and review the generated GraphQL schema: `apollo/schema/generated/prisma.graphql`. Identify a Prisma API call you want to expose to the world. We'll choose `Query.users` for this example.
1. Edit `apollo/schema/apollo.graphql` and import only that query:

    ```graphql
      # import Query.users from "generated/prisma.graphql"
    ```

1. As soon as you save the Apollo schema, `nodemon` will detect the change and will restart Apollo. Nice, right?
1. Now, you need to create a resolver for this type, which you'll do by creating a resolver function in `apollo/src/resolvers/Query.js`

    ```javascript
      const users = (_parent, args, context) => {
        // Pass this call directly through to Prisma and return the result
        return context.prisma.users(args)
      }
    ```

    Note, this is but a simple example. While you may end up just passing many API calls straight through to Prisma, there will also be many times where your Apollo resolvers will contain business logic for data validation, authorization, etc.

At this point, you can continue to iterate between cherry picking Prisma API types and implementing resolvers. The key to this process is that there is a single shared datamodel between Apollo and Prisma, which makes using the Prisma client seamless.

You should _never_ redefine types that are already defined in Prisma, though you may want to extend them or create new types if your Apollo resovers talk to APIs other than Prisma.

## Apollo Make Targets

!!! success "`make apollo-build`"
    Builds the Apollo Docker image

!!! success "`make apollo-push`"
    Builds and pushes the Apollo Docker image to the Docker repository specified by APOLLO_CONTAINER_IMAGE

!!! success "`make apollo-token`"
    Generates a JWT for authenticating to Apollo. Very handy for testing!

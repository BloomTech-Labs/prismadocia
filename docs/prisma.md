# The Prisma Layer

1. Edit your Prisma data model and seeds:
    - `prisma/prisma-datamodel.graphql`
    - `prisma/seeds.js`
2. Deploy your data model changes
    - `make local-prisma-deploy`
3. Get a Prisma token
    - `make local-prisma-token`
4. Open the Prisma playground
    - <http://localhost:7000>
5. Open the 'HTTP Headers' setting in the GraphQL Platground and set your token, like this:
```json
{
    "Authorization":"Bearer <The token from 'make local-prisma-token'>"
}
```

## Prisma Make targets

!!! success "`make local-prisma-deploy`"
    Deploys data model changes to your local Prisma service.

    Note: Prisma will need to be running locally for this to work (e.g. "`make local-up`")

!!! success "`make local-prisma-deploy-force`"
    Prisma will refuse to deploy changes if it can't figure out how to preserve existing data. In those cases, you can use this target to force Prisma to make the changes... or you can just always use this target because you're using fake data anyway, right?

!!! success "`make local-prisma-reseed`"
    Clear out any existing data and will rerun seeding to give you a fresh cut of data.

!!! success "`make local-prisma-token`"
    Generate a token for communicating with your local Prisma API.

    Open the 'HTTP Headers' setting in the GraphQL Platground and set your token, like this:
    ```json
    {
        "Authorization":"Bearer <The token from 'make local-prisma-token'>"
    }
    ```

!!! success "`make prisma-generate`"
    Generates a JavaScript client (with types!) and a GraphQL schema that reflects the Prisma API.

    Files are generated here:

    - `/apollo/schema/generated/prisma.graphql`
    - `/apollo/src/generated/prisma-client`

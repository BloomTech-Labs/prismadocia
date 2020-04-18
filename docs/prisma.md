# The Prisma Layer

## Quickstart

1. Install some tools
      - [Yeoman](https://yeoman.io/)
      - [Docker](https://www.docker.com/)
      - [AWS CLI v2+](https://aws.amazon.com/cli/)

1. Install the Yeoman generator for Prismatopia
``` bash
    yarn global add generator-prismatopia
```

1. Create your new project
``` bash
    yo prismatopia
```

1. Start Prismatopia locally
``` bash
    make local-up
```

1. Deploy the data model
```bash
    make local-prisma-deploy
```

1. Get a token for your local Prisma
```bash
    make local-prisma-token
```

1. Open the Prisma playground
    - <http://localhost:7000>

1. Open the 'HTTP Headers' setting in the GraphQL Platground and set your token, like this:
``` json
    {
      "Authorization":"Bearer <The token from 'make local-prisma-token'>"
    }
```

1. Run some queries!

## Prisma Development

1. Edit your Prisma data model and seeds:
    - `prisma/prisma-datamodel.graphql`
    - `prisma/seeds.js`
1. Deploy your data model changes
    - `make local-prisma-deploy`
1. Get a Prisma token
    - `make local-prisma-token`
1. Open the Prisma playground
    - <http://localhost:7000>
1. Open the 'HTTP Headers' setting in the GraphQL Platground and set your token, like this:
```json
    {
    "Authorization":"Bearer <The token from 'make local-prisma-token'>"
    }
```

## Local Prisma Make targets

### `make local-prisma-deploy`

Deploys data model changes to your local Prisma service.

!!! tip
    Prisma will need to be running locally for this to work (e.g. "`make local-up`")

### `make local-prisma-deploy-force`

Prisma will refuse to deploy changes if it can't figure out how to preserve existing data. In those cases, you can use this target to force Prisma to make the changes... or you can just always use this target because you're using fake data anyway, right?

### `make local-prisma-reseed`

Clear out any existing data and will rerun seeding to give you a fresh cut of data.

### `make local-prisma-token`

Generate a token for communicating with your local Prisma API.

- Open the 'HTTP Headers' setting in the GraphQL Platground and set your token, like this:
   ```json
      {
        "Authorization":"Bearer <The token from 'make local-prisma-token'>"
      }
   ```

### `make prisma-generate`

Generates a JavaScript client (with types!) and a GraphQL schema that reflects the Prisma API.

Files are generated here:

- `/apollo/schema/generated/prisma.graphql`
- `/apollo/src/generated/prisma-client`

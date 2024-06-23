## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.

## Emulate Azure Static Web Application locally

Copy the connection string from one of the [supported database](https://learn.microsoft.com/en-us/azure/static-web-apps/database-overview#supported-databases) in Azure and set to the environment variable:

```bash
export DATABASE_CONNECTION_STRING='<YOUR_CONNECTION_STRING>'
```

To add the environment permanently, create a `.env` file with the following content:

```
DATABASE_CONNECTION_STRING='<YOUR_CONNECTION_STRING>'
```

The build configuration needs to be updated in the `svelte.config.js` file. Most importantly, the `fallback` section has to set to the `index.html` value. Once you configured the SWA database connection by following the appropriate [Tutorials](https://learn.microsoft.com/en-us/azure/static-web-apps/database-azure-cosmos-db?tabs=bash) under the Database connection section in the Azure Static Web Apps Documentation, start the static web app with it:

```bash
npx swa start ./build --data-api-location swa-db-connections
```

Go to the <http://localhost:4280> URL that is under the `host.cors.origins` section in the `swa-db-connections/staticwebapp.database.config.json` configuration.

> Note: As you can see, the `./build` folder is selected to start the static web application.

To change other container in the NoSQL database, adapt the following files:

- `staticwebapp.database.config.json`: Replace the database in the `data-source.options.database` and the `entities` section.
- `staticwebapp.database.schema.gql`: Modify the database schema to match with the selected container.
- `+page.svelte`: Adapt the CRUD operation according to the new database schema.

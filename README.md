# A minimal Reproduction for Nuxt x Sanity X Firebase Presentation Tool Issue

## Prerequisities

- [Node.js](https://nodejs.org/en/) (v20.10.0)
- [Sanity CLI](https://www.sanity.io/docs/getting-started-with-sanity-cli)

## Getting started

This project is a monorepo that consist of two parts.

- root: contains the main nuxt app
- `app/studio`: contains the sanity studio

You can also double check in `pnpm-workspace.yaml`.

This starter uses [Nuxt](https://nuxt.com/) for the front end and [Sanity](https://www.sanity.io/) to handle its content.

Run the following commands to prepare both applications, each step should be executed from the **root directory**:

1. Install dependencies.

```sh
pnpm install
```

2. Select or create a Sanity project and dataset, with your own account

3. [Generate a token](https://www.sanity.io/docs/http-auth#4c21d7b829fe) with read permissions for use in the next step.

4. Copy the example .env file (`.env.example`) and populate it with the required values in corresponding folders (because we have 2 `.env` files, in root and `app/studio`).

5.  Start the development servers:

In order to start developing you need to run two commands, one for the nuxt dev server and one other for the sanity studio. But for the best experience, you can run both of them in parallel with the following command:

```bash
pnpm dev:all
```

- Your Nuxt app should now be running on [http://localhost:3000/](http://localhost:3000/).
- Your Studio should now be running on [http://localhost:3333/](http://localhost:3333/).

## Check Presentation Tool in Local

To be able to reproduce the issue, first you need to make sure that the prensentation tools works in local. Make sure `SANITY_STUDIO_PREVIEW_URL` is empty in your `.env` file to be able to connect the sanity to your local server

1. Add a dummy post to make our app displays some data via `http://localhost:3333`
2. After starting development servers, open `http://localhost:3333/presentation` and make sure that it connects with the local nuxt app and you are able to make changes to the content via presentation mode.

## Deployments

Here will be divided into 2 parts: Nuxt app deployment and Sanity deployment

### Nuxt app deployment

Make sure you already installed `firebase-tools` by `pnpm install -g firebase-tools`

1. Make firebase projects with your own account, and make it with the [Blaze projects, to be able to make SSR app](https://nitro.build/deploy/providers/firebase)

2. Connect you firebase project into the app by making both `.firebaserc` and `firebase.json` in the root folder based on example files (`.firebaserc.example` and `firebase.json.example`)

or you can also simply running below command, even though I suggest to use the first one because it's easier and you don't have to start from scratch 
```bash
firebase init
``` 

3. Build packages for nuxt app
```bash
pnpm build
```

6. Make sure `SANITY_STUDIO_PREVIEW_URL` env variable already replaced by your own production url, in my case would be

- `NUXT_SANITY_STUDIO_URL` = `https://<your-sanity-host-domain-origin>.sanity.studio/`


4. Deploy your SSR Nuxt app
```bash
pnpm deploy
```

or if you got error `ERR_PNPM_NOTHING_TO_DEPLOY`, you can just directly run the command
```bash
firebase deploy
```

5. Make sure that your app is available via `<your-firebase-project-id>.web.app` or the url that appear in the end after we deploy our app in firebase via terminal

### Sanity deployment
Make sure `SANITY_STUDIO_PREVIEW_URL` env variable already replaced by your own production url, in my case would be
- `SANITY_STUDIO_PREVIEW_URL` = `https://<your-firebase-project-id>.web.app/`

1. Replace `studioHost` in `sanity.cli.ts` by your chosen sanity domain origin (`)

2. Build your sanity project in `app/studio` folder
```bash
pnpm build
```

3. Deploy your sanity project
```bash
pnpm deploy
```

or you can directly command if you got any error running above command
```bash
sanity deploy
```

4. Verify that your sanity already deployed in `<your-sanity-host-domain-origin>.sanity.studio`
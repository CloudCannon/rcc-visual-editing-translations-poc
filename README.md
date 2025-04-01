# Astro Rosey Starter

This is an **experimental** proof of concept demonstrating using the Rosey CloudCannon Connector, with visual translations.

Translations are stored on each page as frontmatter, and kept in sync with the translation data files that would normally be present with the [RCC](https://github.com/CloudCannon/rcc). Keeping the translations stored on each page means we can make changes to translations and visually preview them on the page they'll actually be used on.

## Requirements

- A CloudCannon organisation with access to [publishing workflows](https://cloudcannon.com/pricing/).
- A smartling account if needed

## Environment Variables

The `SYNC_PATHS=/rosey/` environment variable tells CloudCannon to sync the generated data in the `/rosey/` directory from our postbuild back to the source repository. We only want this to happen on staging.

The `TRANSLATE=true` environment variable tells Rosey to generate a site from the data in the locales folder.
Only set this to `true` on the production site. We also don't run the generate scripts if `TRANSLATE=true`, as the locales we generate the multilingual site from are already in place, meaning we don't need to run it again. It has the added bonus of meaning we only need the one environment variable on our production site.
Setting this to `true` on our staging site will interfere with CloudCannon's UI, and means we can't enter translations, or edit pages. This is why we need a staging to production publishing workflow.

To use smartling, your user id, and project id need to be added via the config file. Then your secret API key can be added to your staging site's (where the workflow runs) environment variables as `DEV_USER_SECRET`.

## Adding Translations

Add a tag of `data-rosey="example-key"` to an HTML element containing text, with a key for Rosey to use to keep track of that piece of text content.

Tag values need to be unique for Rosey to keep track of what translations go where, so you will need to generate an id as demonstrated in this template. We can use a `generateRoseyId()` function to slugify the content of the translation and use that as the Rosey key. See [below](#bookshop) for more information.

Once we rebuild the site, we can see inputs for our translations on `staging`. Once we enter a translation, save and wait for the build to finish, we can publish to our `main` branch, and Rosey will our translations to generate a multilingual site for us.

To create your own components that add inputs to our translation files, add a `data-rosey=""` tag, following a format similar to the one provided in the placeholder components.
## Local Development & Testing

To run site locally:

```bash
npm i
npm start
```

To run Rosey locally (handy for debugging):

```bash
npm run build
./.cloudcannon/postbuild
```

If you get a permission error for running the postbuild locally, you can try changing the permissions for that file with:

```bash
chmod u+x ./.cloudcannon/postbuild
```

### Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

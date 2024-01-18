---
id: index
title: Building Frontend Apps
sidebar_label: Overview
# prettier-ignore
description: Building frontend apps using the new frontend system
---

> **NOTE: The new frontend system is in alpha and is only supported by a small number of plugins.**

To get set up quickly with your own Backstage project you can create a Backstage App.

A Backstage App is a monorepo setup that includes everything you need to run Backstage in your own environment.

If you intend to contribute a plugin, new feature, or bug fix to the Backstage project, you may want to read the Contributors guide instead.

## Creating a new app

To create a Backstage app, you will need to have Node.js Active LTS Release installed.

Backstage provides a utility for creating new apps. It guides you through the initial setup of selecting the name of the app and a database for the backend. The database options are either SQLite or PostgreSQL, where the latter requires you to set up a separate database instance. If in doubt, choose SQLite, but don't worry about the choice, it's easy to change later! Here is a tutorial for it.

The easiest way to run the create app package is with npx:

```sh
npx @backstage/create-app@latest
```

This will create a new Backstage App inside the current folder. The name of the app-folder is the name that was provided when prompted.

:::note
The created app will currently be templated for use in the legacy frontend system, and you will need to replace the existing app wiring code.
:::

## The app instance

The starting point of a frontend app is the `createApp` function, which accepts a single options object as its only parameter. It is imported from `@backstage/frontend-app-api`, which is where you will find most of the common APIs for building apps.

This is how to create a minimal app:

```tsx title="in src/index.ts"
import ReactDOM from 'react-dom/client';
import { createApp } from '@backstage/frontend-app-api';

// Create your app instance
const app = createApp({
  // Features such as plugins can be installed explicitly, but we will explore other options later on
  features: [catalogPlugin],
});

// This creates a React element that renders the entire app
const root = app.createRoot();

// Just like any other React we need a root element. No server side rendering is used.
const rootEl = document.getElementById('root')!;
```

Note that the `createRoot` exports an element instead of a component and it no longer accepts a custom root element as parameter, so this is the reason you can pass the returned element directly to React `createRoot`. See [costumizing your root app element](#app-root) for more details on how to do it with extensions.

## Configure your app

### Enable features discovering

This is how you enable the experimental feature discovering when building your app with the `@backstage/cli`, check out [here](https://backstage.io/docs/frontend-system/architecture/app#feature-discovery) for more details.

:::warning
Remember that package extensions that are not auto-discovered must be manually added to the application when creating an app. See [features](#install-features-manually) for more details.
:::

### Bind external routes

This is the configuration you do when you want to associate an external plugin route ref with a regular one. There are two ways of doing it: via configuration file or through code. Access [this](https://backstage.io/docs/frontend-system/architecture/routes#binding-external-route-references) page for learning both ways.

## Customize individual extensions

There is a section on the `app-config.yaml` file that is reserved for enabling, disabling, ordering or configuring extensions individually. Use this for any type of extension: app, plugin or standalone extensions. To get familiar with what what is available for app extensions, go to the [built-in extensions](./02-built-in-extensions.md) documentation. For plugins, we recomend you checking their read files documentation.

## Install features manually

A manual installation is necessary in case your features aren't auto discovered eather because your are not using the `@backstage/cli` to build your app or when the features are defined in local modules in the app package. A manual installation requires you to write code, so you have to import your features and pass then to the `createApp` function:

```tsx title="packages/app/src/App.tsx"
import { createApp } from '@backstage/frontend-app-api';
// This plugin was create as a local module in the app
import { somePlugin } from './plugins';

const app = createApp({
  features: [somePlugin],
});

export default app.createRoot();
```

:::info
You can also pass overrides to the features array, for more details, please read the [extension overrides](../architecture/05-extension-overrides.md) documentation.
:::

If for some reason you need to perform asynchronous operations before passing features to the createApp function, you can use an async function as `features` option and this should return a features array promise:

```tsx title="packages/app/src/App.tsx"
import { createApp } from '@backstage/frontend-app-api';

const app = createApp({
  // Lazy loading the plugin feature
  features: import('./plugins/MyPlugin').then(m => [m.default]),
});

export default app.createRoot();
```

## Lazy load your configuration file

In some cases we want to load our configuration from a backend server or we just want to override the default app configuration loader. If you want to do it, you can pass an async callback to the `createApp` function and it should return a promise of the config object:

```tsx title="packages/app/src/App.tsx"
import { createApp } from '@backstage/frontend-app-api';

const app = createApp({
  // Lazy loading the plugin feature
  features: import('./plugins/MyPlugin').then(m => [m.default]),
});

export default app.createRoot();
```

## Customize or override built-in extensions

Previously you would costumize the app route, components, apis, sidebar and etc via code in the `App.tsx`. Now we want you to write less code and install more extensions to costumize your backstage instance. See below what type of extensions are available for you to costumize your app.

---
id: built-in-extensions
title: App Built-in Extensions
sidebar_label: Built-in extensions
# prettier-ignore
description: Configuring or overriding built-in extensions
---

Built-in extensions are default app extensions that are always installed when you create an Backstage app.

## Disable built-in extensions

All built-in elements can be disabled in the same way as you disable any other extension:

```yaml title="app-config.yaml"
extensions:
  # Disabling the built-in app root alert element
  - app-root-element:app/alert-display: false
```

:::warning
Be careful when disabling built-in extensions, as there may be other extensions depending on their existence. For example, the built-in "alert display" extension displays messages retrieved via [AlertApi](https://backstage.io/docs/reference/core-plugin-api.alertapi) and disabling this extension will cause the application to no longer display these messages unless you install another extension that displays messages from `AlertApi`.
:::

## Override built-in extensions

You can override any built-in extension whenever their customizations, whether through configuration or input, do not meet a use case for your Backstage instance. Check out [this](../architecture/05-extension-overrides.md) documentation on how to override application extensions.

:::warning
Be aware there could be some implementation requirements to properly override an built-in extension, such as using same apis and do not remove inputs or configurations otherwise you can cause a side effect in other parts of the system that expects same minimal behavior.
:::

## Default built-in extensions

## App root

This is the extension that creates the app root element, so it renders root level components such as app router, layout. As any extension, the app root one can also be customized via inputs, see the input's list below:

### Router component

<!-- TODO: add the App Root Route type in the extension types --->

An optional React Router component that should manage the app routes context. The default value of this input is a Browser router but you can change it to a custom component by installing an extension of App Root Router type, see this documentation for more details on how to create a custom root app router extensions.

### Sign-in page

An optional React component that should render an application login page that unauthenticated users will be redirected to after accessing a page that requires authentication. The default `AppRoot` extension does not use a default component for this input, it bypasses the user authentication check and always renders all routes when a login page is not installed.

Use the `createSignInPageExtension` to extend Backstage with Sign-in page, checkout the factory [Sign-in Page Extension Type](https://backstage.io/docs/frontend-system/building-plugins/extension-types#signinpage---reference) for more details.

:::warning
Make sure to call the `onSignInSuccess` prop when the user has been successfully authorized, otherwise the user will not be correctly redirected to the application home page.
:::

### Children

It is an optional React component that should render the app sidebar and main content, so this component is the one that defines the app skeleton or layout. There is a [default app layout extension]

### Elements

The root app extension can optionally receive React elements to be redered outside of the app layout, such as shared popups. These elements are received as inputs and there are two built-in elements that are always installed in the app: An alert display an oauth request modal.

#### Alert Display

It is a app root element extension and it displays messages posted via the `AlertApi` ([here](https://backstage.io/docs/reference/core-plugin-api.alertapi) is the documentation for using this api in your components).

There is no api referece for built-in extensions, see below what configurations are available to customize the `Alert Display` output:

| Key                | Type                                                                       | Default value                             | Description                                                      |
| ------------------ | -------------------------------------------------------------------------- | ----------------------------------------- | ---------------------------------------------------------------- |
| transientTimeoutMs | number                                                                     | 5000                                      | Time in miliseconds to wait before displaying messages           |
| anchorOrigin       | { vertical: 'top' \| 'bottom', horizontal: 'left' \| 'center' \| 'right' } | { vertical: 'top', horizontal: 'center' } | Position on the screen where the message alert will be displayed |

#### OAuth Request Dialog

#### Create your own root elements

If you would like to render more elements that should be available at app root level, create app root elements extensions. Here is the documentation.

### App components

<!--
Explain the concept of app components and when use it in comparison to extension inputs.
-->

#### Defaults components

<!--
Write about the default app components: progress, not found page and error boundary fallback component.
-->

### Custom components

<!--
Write an example creating custom app component using the `createComponentExtension` and link to the factory api reference page.
-->

### App theme

<!--
Briefly write what are themes and list default themes installed by default in the app.

Elaborate an example on how to create a custom theme using the `createThemeExtension` and link to the factory api reference page.
-->

### App layout

<!--
Briefly write what this extension does and how to override it.
-->

### App sidebar

<!--
Briefly write what this extension does.

Elaborate an example on how to add items and change the logo using the `createNavItemExtension` and `createNavLogoExtension`. Don;t forget linking to the factory api reference page.
-->

#### Overriding

<!-- Create and example overriding the app sidebar -->

### App router

<!--
Briefly write what this extension does.

Elaborate an example on how to use a different router component with the `createRouterExtension` helper and link to the factory api reference page.

Elaborate an example on how to use a different sign-in page component with the `createSignInPageextension` helper and link to the factory api reference page.
-->

### App apis

<!--
Explain the concept of app apis.

Elaborate an example on how to create an api using the `createApiExtension` creator and link to the factory api reference page.
-->

### App routes

<!--
Explain the concept of app routes.

Elaborate an example on how to create an route using the `createPageExtension` creator and link to the factory api reference page.
-->

### App translation

<!--
Explain the concept of app translations.

Elaborate an example on how to create an translation using the `createTranslatorExtension` creator and link to the factory api reference page.
-->

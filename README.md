<div align="center">
  <img src="https://avatars.githubusercontent.com/u/92730708?s=96&v=4" /> 
  <h1>Angular 13 Template</h1>
</div>

<p align="center">
  <a href="https://mia-platform.eu/?utm_source=referral&utm_medium=github&utm_campaign=micro-lc">
    <img src="https://img.shields.io/badge/Supported%20by-Mia--Platform-green?style=for-the-badge&link=https://mia-platform.eu/&color=DE0D92&labelColor=214147" alt="Mia-Platform"/>
  </a>
</p>

<p align="center">
  <a href="https://github.com/micro-lc/angular13-template/actions">
    <img src="https://github.com/micro-lc/angular13-template/workflows/Main%20CI/badge.svg" alt="Build Status" />
  </a>

  <a href="https://coveralls.io/github/micro-lc/angular13-template?branch=main">
    <img src="https://coveralls.io/repos/github/micro-lc/angular13-template/badge.svg?branch=master" alt="Coverage Status" />
  </a>
</p>

Boilerplate for an Angular 13 [micro-lc parcel](https://micro-lc.io/docs/guides/applications/parcels). It implements the
necessary features to work both standalone and in micro-lc.

## Local development

First thing you need to do is install the dependencies. Enable Yarn running 

```sh
corepack enable
```

(or install it as a global dependency with `npm i -g yarn` for Node < 16.9.0), and run 

```sh
yarn install
```

> **Note**
>
> If you whish to use NPM instead of Yarn, simply delete the `yarn.lock` file and run `npm install`.

Once you have the dependencies in place, run

```sh
yarn start
```

to spin up the application.

Tests can be run with

```sh
yarn coverage
```

### Build

To make the internal routing work in micro-lc, a custom script that removes the `<base>` tag from the generated HTML has
to be executed at the end of the build process.

If you don't whish to execute this step, simply change the build script in the `package.json`:

```diff
- 8 "build": "ng build && node plugins/post-html.js",
+ 8 "build": "ng build",
```

> **Warning**
>
> If you change the output directory, remember to change line 4 of the script (located in `plugins/post-html.js`) accordingly.

## Use in micro-lc

Applications build with this template can be used as-is in micro-lc as [parcels](https://micro-lc.io/docs/guides/applications/parcels).

An example configuration may be:

```json5
{
  "applications": {
    "angular13-parcel": {
      "integrationMode": "parcel",
      "route": "./angular13-parcel/", // <-- must have the ending "/", should have the starting "."
      "entry": "/my-micro-lc-angular13-parcel/", // <-- must have the ending "/"
      "injectBase": true // <-- must be "true" if hash routing is not used
    }
  }
}
```

### Internal routing

The internal routing of the application is already set up to work in micro-lc, meaning that the base url of the internal
routes is dynamically computed on the bases of micro-lc `<base>`, as explained in the 
[official documentation](https://micro-lc.io/docs/guides/applications/parcels/#injectbase).

> **Note**
>
> If you whish to use a hash router in your application, change `app-routing.module.ts` file as such:
> 
> ```diff
> - 17 imports: [RouterModule.forRoot(routes)],
> + 17 imports: [RouterModule.forRoot(routes, { useHash: true })],
> 
> - 19 providers: [{ provide: APP_BASE_HREF, useValue: baseUrl() }]
> + 19
> ```

### Assets

To correctly load assets in micro-lc, they should be put in `src/assets/` folder, and referenced using the `assetUrl`
[pipe](https://angular.io/guide/glossary#pipe).

A running example can be found in the `Home` component:

```html
<img alt="logo" class="app-logo" [src]="'angular-logo.svg' | assetUrl" />
```

### `zone.js`

Angular applications need [`zone.js`](https://github.com/angular/angular/tree/main/packages/zone.js) library to run, which
is not bundled in micro-lc. Therefore, you need to import it as a `<script>` in you micro-lc entrypoint **before** any
micro-lc related module.

```html
<!DOCTYPE html>
<html lang="en">

<head>

  [...]

  <script type="module" src="https://cdn.jsdelivr.net/npm/zone.js@0.13.0/dist/zone.min.js"></script>

  <script type="module" src="https://cdn.jsdelivr.net/npm/@micro-lc/orchestrator@latest/dist/micro-lc.production.js"></script>
</head>

[...]

</html>
```

> **Warning**
>
> `zone.js` is also imported in the entrypoint of this application to make it work in development mode. Whereas it is
> advisable to remove it before bundling for production, the import it can be kept as long as the version matches the one
> imported in micro-lc.

---

## DevOps console

This walkthrough will explain you how to correctly create a [micro-lc](https://www.micro-lc.io) Angular 13 plugin from the DevOps Console.

### Create a microservice

Access your [Mia-Platform DevOps Console](https://console.cloud.mia-platform.eu/login), create a new project, and go to the **Design** area.

From the Design area of your project, select _Microservices_ and then create a new one, you have now reached [Mia-Platform Marketplace](https://docs.mia-platform.eu/development_suite/api-console/api-design/marketplace/)!
In the marketplace you will see a set of Examples and Templates that can be used to set-up microservices with a predefined and tested function.

For this walkthrough select the following template: **micro-lc Angular 13 Plugin Template**.
Give your microservice the name you prefer, in this walkthrough we'll refer to it with the following name: **micro-lc-angular13-plugin-template**. Then, fill the other required fields and confirm that you want to create a microservice.  
A more detailed description on how to create a Microservice can be found in [Microservice from template - Get started](https://docs.mia-platform.eu/development_suite/api-console/api-design/custom_microservice_get_started/#2-service-creation) section of Mia-Platform documentation.

### Expose an endpoint to your microservice

In order to access to your new microservice it is necessary to create an endpoint that targets it.  
In particular, in this walkthrough you will create an endpoint to your microservice *micro-lc-angular13-plugin-template*. To do so, from the Design area of your project select _Endpoints_ and then create a new endpoint.
Now you need to choose a path for your endpoint and to connect this endpoint to your microservice. Give to your endpoint the following path: **/micro-lc-angular13**. Then, specify that you want to connect your endpoint to a microservice and, finally, select *micro-lc-angular13-plugin-template*.  
Step 3 of [Microservice from template - Get started](https://docs.mia-platform.eu/development_suite/api-console/api-design/custom_microservice_get_started/#3-creating-the-endpoint) section of Mia-Platform documentation will explain in detail how to create an endpoint from the DevOps Console.

### Save your changes

After having created an endpoint to your microservice you should save the changes that you have done to your project in the DevOps console.  
Remember to choose a meaningful title for your commit (e.g 'created service micro_lc_angular13_plugin'). After some seconds you will be prompted with a popup message which confirms that you have successfully saved all your changes.  
Step 4 of [Microservice from template - Get started](https://docs.mia-platform.eu/development_suite/api-console/api-design/custom_microservice_get_started/#4-save-the-project) section of Mia-Platform documentation will explain how to correctly save the changes you have made on your project in the DevOps console.

### Deploy

Once all the changes that you have made are saved, you should deploy your project through the DevOps Console. Go to the **Deploy** area of the DevOps Console.  
Once here select the environment and the branch you have worked on and confirm your choices clicking on the *deploy* button. When the deploy process is finished you will receive a pop-up message that will inform you.  
Step 5 of [Microservice from template - Get started](https://docs.mia-platform.eu/development_suite/api-console/api-design/custom_microservice_get_started/#5-deploy-the-project-through-the-api-console) section of Mia-Platform documentation will explain in detail how to correctly deploy your project.

### Try it

Now, if you launch the following command on your terminal (remember to replace `<YOUR_PROJECT_HOST>` with the real host of your project):

```shell
curl <YOUR_PROJECT_HOST>/micro-lc-angular13
```

you should see a Angular-based webpage.

Congratulations! You have successfully learnt how to use our _micro-lc Angular 13 Plugin_ Template on the DevOps Console!

Angular CLI is a Command Line Interface (CLI) to automate your development workflow. It allows you to:

*   create a new Angular application
*   run a development server with LiveReload support to preview your application during development
*   add features to your existing Angular application
*   run your application’s unit tests
*   run your application’s end-to-end (E2E) tests
*   build your application for deployment to production

Angular CLI does not just blindly generate code for you. It uses static analysis to better understand the semantics of your application. For example, when adding a new component using `ng generate component`, Angular CLI finds the closest module in the module tree of your application and integrates the new feature in that module. So if you have an application with multiple modules, Angular CLI will automatically integrate the new feature in the correct module, depending on the directory where you run the command from.

### Installing Angular CLI

```bash
$ npm install -g @angular/cli
```

## Creating and Runnning a New Angular Application

There are two ways to create a new application using Angular CLI:

*   `ng init`: create a new application in the current directory
*   `ng new`: create a new directory and run `ng init` inside the new directory

So `ng new` is similar to `ng init` except that it also creates a directory for you.


```bash
$ ng new my-app
```

**Options:**

*   `--dry-run`: boolean, default `false`, perform dry-run so no changes are written to filesystem
*   `--verbose`: boolean, default `false`
*   `--link-cli`: boolean, default `false`, automatically link the `@angular/cli` package ([more info](https://github.com/angular/angular-cli#development-hints-for-hacking-on-angular-cli))
*   `--skip-install`: boolean, default `false`, skip `npm install`
*   `--skip-git`: boolean, default `false`, don’t initialize git repository
*   `--skip-tests`: boolean, default `false`, skip creating tests
*   `--skip-commit`: boolean, default `false`, skip committing the first git commit
*   `--directory`: string, name of directory to create, by default this is the same as the application name
*   `--source-dir`: string, default `'src'`, name of source directory
*   `--style`: string, default `'css'`, the style language to use (`'css'`, `'less'` or `'scss'`)
*   `--prefix`: string, default `'app'`, the prefix to use when generating new components
*   `--mobile`: boolean, default `false`, generate a Progressive Web App application
*   `--routing`: boolean, default `false`, add module with routing information and import it in main app module
*   `--inline-style`: boolean, default `false`, use inline styles when generating the new application
*   `--inline-template`: boolean, default `false`, use inline templates when generating the new application

**To run the app:**

```bash
$ cd my-app
$ ng serve
```

## Adding Features

*   `ng generate class my-new-class`: add a class to your application
*   `ng generate component my-new-component`: add a component to your application
*   `ng generate directive my-new-directive`: add a directive to your application
*   `ng generate enum my-new-enum`: add an enum to your application
*   `ng generate module my-new-module`: add a module to your application
*   `ng generate pipe my-new-pipe`: add a pipe to your application
*   `ng generate service my-new-service`: add a service to your application

The `generate` command and the different sub-commands also have shortcut notations, so the following commands are similar:

*   `ng g cl my-new-class`: add a class to your application
*   `ng g c my-new-component`: add a component to your application
*   `ng g d my-new-directive`: add a directive to your application
*   `ng g e my-new-enum`: add an enum to your application
*   `ng g m my-new-module`: add a module to your application
*   `ng g p my-new-pipe`: add a pipe to your application
*   `ng g s my-new-service`: add a service to your application

Run `$ ng generate --help` to see all available options of your locally installed Angular CLI.

### Adding a new Class

```bash
$ ng generate class user-profile
```

**Options:**

*   `--spec`: boolean, default `false`, generate spec file with unit test

### Adding a new Component

```bash
$ ng generate component site-header
```

**Options:**

*   `--flat`: boolean, default `false`, generate component files in `src/app` instead of `src/app/site-header`
*   `--inline-template`: boolean, default `false`, use an inline template instead of a separate HTML file
*   `--inline-style`: boolean, default `false`, use inline styles instead of a separate CSS file
*   `--prefix`: boolean, default `true`, use prefix specified in `.angular-cli.json` in component selector
*   `--spec`: boolean, default `true`, generate spec file with unit test
*   `--view-encapsulation`: string, specifies the view encapsulation strategy
*   `--change-detection`: string, specifies the change detection strategy

### Adding a new Directive

```bash
$ ng generate directive admin-link
```

**Options:**

*   `--flat`: boolean, default `true`, generate directive files in `src/app` instead of `src/app/admin-link`
*   `--prefix`: boolean, default `true`, use prefix specified in `.angular-cli.json` in directive selector
*   `--spec`: boolean, default `true`, generate spec file with unit test

### Adding a new enum

```bash
$ ng generate enum direction
```

### Adding a new Module

```bash
$ ng generate module admin
```

**Options:**

*   `--routing`: boolean, default `false`, generate an additional module `AdminRoutingModule` with just the routing information and add it as an import to your new module
*   `--spec`: boolean, default `false`, add `src/app/admin/admin.module.spec.ts` with a unit test that checks whether the module exists

### Adding a new Pipe

```bash
$ ng generate pipe convert-to-euro
```

**Options:**

*   `--flat`: boolean, default `true`, generate component files in `src/app` instead of `src/app/convert-to-euro`
*   `--spec`: boolean, default `true`, generate spec file with unit test

### Adding a new Service

```bash
$ ng generate service backend-api
```

**Options:**

*   `--flat`: boolean, default `true`, generate service file in `src/app` instead of `src/app/backend-api`
*   `--spec`: boolean, default `true`, generate spec file with unit test

## Tests

### Running Unit Tests

```bash
$ ng test
```

### Running End-to-end (E2E) Tests

```bash
$ ng e2e
```

## Building App for Production

Running `ng serve` builds and bundles your Angular application automatically to a virtual filesystem during development.

However, when your application is ready for production, you will need real files that you can deploy to your server or to the cloud.

To build and bundle your application for deployment, run:


```bash
$ ng build
```

**Options:**

*   `--aot`: enable ahead-of-time compilation
*   `--base-href`: string, the base href to use in the index file
*   `--environment`: string, default `dev`, environment to use
*   `--output-path`: string, directory to write the output to
*   `--target`: string, default `development`, environment to use
*   `--watch`: boolean, default `false`, watch files for changes and rebuild when a change is detected

### Targets

Specifying a target impacts the way the build process operates. Its value can be one of the following:

*   `development`: default mode, do not minify or uglify code
*   `production`: minify and uglify code

### Environments

Environments let you specify settings to customize your application behavior.

You can define your own environments in the `.angular-cli.json` file. The default ones are:

*   `source`: use settings defined in `environments/environment.ts`
*   `dev`: use settings defined in `environments/environment.ts`
*   `prod`: use settings defined in `environments/environment.prod.ts`

The Angular CLI section is an excerpt from [www.sitepoint.com/ultimate-angular-cli-reference/](https://www.sitepoint.com/ultimate-angular-cli-reference/) by Jurgen Van de Moere ([@jvandemo](https://twitter.com/jvandemo)).


# Table of contents


   <br>
   
### *Session 1*

### __Angular__ is an application design framework and development platform for creating efficient and sophisticated single-page apps.

<br>
Angular is a development platform, built on TypeScript. As a platform, Angular includes:

* A component-based framework for building scalable web applications

* A collection of well-integrated libraries that cover a wide variety of features, including routing, forms management, client-server communication, and more

* A suite of developer tools to help you develop, build, test, and update your code

# Visual studio code

TO DO...

# Setup CLI

### Prerequisites
* **Node JS**

    Angular requires an [active LTS or maintenance LTS](https://nodejs.org/about/releases) version of Node.js.

* **npm package manager**
    Angular, the Angular CLI, and Angular applications depend on `npm packages` for many features and functions. To download and install npm packages, you need an npm package manager. This guide uses the npm client command line interface, which is installed with `Node.js` by default. To check that you have the `npm client` installed, run `npm -v` in a terminal window.

Major versions of Angular CLI follow the supported major version of Angular, but minor versions can be released separately.

Install the CLI using the `npm` package manager:

```
npm install -g @angular/cli
```

To list all commands or get help on any command use below command:

```
ng help
ng generate --help
```

To create, build, and serve a new, basic Angular project on a development server, go to the parent directory of your new workspace use the following commands:

```
ng new my-first-project
cd my-first-project
ng serve
```

In your browser, open http://localhost:4200/ to see the new application run. When you use the ng serve command to build an application and serve it locally, the server automatically rebuilds the application and reloads the page when you change any of the source files.

# CLI command-language syntax

Command syntax is shown as follows:

`ng` *commandNameOrAlias* *requiredArg* [*optionalArg*] [`options`]

* Most commands, and some options, have aliases. Aliases are shown in the syntax statement for each command.

* Option names are prefixed with a double dash (--). Option aliases are prefixed with a single dash (-). Arguments are not prefixed. For example:

    ```
    ng build my-app -c production
    ```

* Typically, the name of a generated artifact can be given as an argument to the command or specified with the --name option.

* Argument and option names can be given in either camelCase or dash-case. `--myOptionName` is equivalent to `--my-option-name`.`

## ng-add
Adds support for an external library to your project.

```
ng add <collection> [options]
```

Adds the npm package for a published library to your workspace, and configures the project in the current working directory to use that library, as specified by the library's schematic. For example, adding `@angular/pwa` configures your project for PWA support:

```
ng add @angular/pwa
```

## ng-config
Retrieves or sets Angular configuration values in the angular.json file for the workspace.

```
ng config <json-path> <value> [options]
```

## ng-generate
Generates and/or modifies files based on a schematic.

```
ng generate <schematic> [options]
```

* class
    ```
    ng generate class <name> [options]
    ```
* module
    ```
    ng generate module <name> [options]
    ```
* component
    ```
    ng generate component <name> [options]
    ```
* directive
    ```
    ng generate directive <name> [options]
    ```
* service
    ```
    ng generate service <name> [options]
    ```
* guard
    ```
    ng generate guard <name> [options]
    ```
* interceptor
    ```
    ng generate interceptor <name> [options]
    ```

# Development commands

## ng build
Compiles an Angular app into an output directory named dist/ at the given output path. Must be executed from within a workspace directory.

```
ng build <project> [options]
```

The application builder uses the webpack build tool, with default configuration options specified in the workspace configuration file (angular.json) or with a named alternative configuration. A "development" configuration is created by default when you use the CLI to create the project, and you can use that configuration by specifying the --configuration development.

## ng run
Runs an Architect target with an optional custom builder configuration defined in your project.

```
ng run <target> [options]
```

Architect is the tool that the CLI uses to perform complex tasks such as compilation, according to provided configurations. The CLI commands run Architect targets such as `build`, `serve`, `test`, and `lint`. 

Each named target has a default configuration, specified by an "options" object, and an optional set of named alternate configurations in the "configurations" object.

## ng serve

Builds and serves your app, rebuilding on file changes.
```
ng serve <project> [options]
```

## ng test
Runs unit tests in a project.

```
ng test <project> [options]
```

### *End of session 1*
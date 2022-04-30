# Table of contents
1. [Introduction](#angular-is-an-application-design-framework-and-development-platform-for-creating-efficient-and-sophisticated-single-page-apps)
2. [Setting up Development environment](#visual-studio-code)
2. [Setup environment (CLI) ](#setup-cli)
3. [Core ideas](#angular-applications-the-essentials)
   * [Components](#-components-)
   * [Templates](#-templates-)
   * [Dependency injection](#-dependency-injection-)
4. [Try it](try-it.md)
   * [Getting started](try-it.md)
   * [Adding navigation](adding-navigation.md/)
   * [Managing data](managing-data.md/)
   * [Forms for user input](forms-user-input.md/)
5. [Components in detail](components.md)
   * [Lifecycle hooks](components/lifecycle.md)
   * [View encapsulation](components/view-encapsulation)
   * [Component interaction](components/component-interaction.md)
   * [Component styles](components/component-styles.md)
   * [Parent child communication](components/parent-child-communication.md)
6. [Templates in detail](templates/text-interpolation.md)
   * [Text interpolation](templates/text-interpolation.md)
   * [Template statements](templates/template-statements.md)
   * [Pipes](templates/pipes.md)
   * [Property binding](templates/property-binding.md)
   * [Attribute, class, and style bindings](templates/attribute-class-style-bindings.md)
   * [Event binding](templates/event-binding.md)
   * [Two-way binding](templates/two-way-binding.md)
   * [Template reference variables  ](templates/template-reference-variables.md)
   * [SVG as templates](templates/svg-in-templates.md)
7. Directives in detail
   * [Built-in directives](directives/built-in-directives.md)
   * [Attribute Directives](directives/attribute-directives.md)
   * [Structural directives](directives/structural-directives.md)
8. Dependency injection
   * [Dependency injection](dependency-injection/dependency-injection.md)
   * [Dependency injection providers](dependency-injection/dependency-injection.md)
9. [Modules](modules.md)
10. [Routing and navigation](routing-navigation.md)
11. [Http Client](http-client.md)
12. [Reactive forms](reactive-forms.md)
13. [Observables & RxJS](Observables&RxJS/overview.md)
    * [Observables Overview](Observables&RxJS/overview.md)
    * [The RxJS library](Observables&RxJS/rxjslibrary.md)
    * [Observables in Angular](Observables&RxJS/observable-in-angular.md)
    * [Practical observable usage](Observables&RxJS/practical-observable-usage.md)
    * [Compared to other techniques](Observables&RxJS/other-techniques.md)

14. [Tutorial: Tour of Heroes](tour-of-heroes.md)

     [Introduction](tour-of-heroes.md)

     [Create a Project](tourofheroes/create-project.md)
     
     1. [The Hero Editor](tourofheroes/the-hero-editor.md)

     2. [Display a List](tourofheroes/display-list.md)

     3. [Create a feature component ](tourofheroes/create-a-feature-component.md)

     4. [Add Services](tourofheroes/add-service.md)

     5. [ Add Navigation](tourofheroes/add-navigation.md)
     
     6. [Get Data from a Server](tourofheroes/get-data-from-server.md)

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
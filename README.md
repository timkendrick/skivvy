# Skivvy [![experimental](http://badges.github.io/stability-badges/dist/experimental.svg)](http://github.com/badges/stability-badges)

> Lightweight task runner for modular, reusable build systems


## Installation

Install Skivvy globally:

```bash
npm install -g skivvy
```

This will make the `skivvy` command globally available. See the list of [available tasks](#available-tasks) below.


## Why Skivvy?

- Skivvy provides a simple tool that **autoloads tasks and exposes them to the command-line** without you having to write a single line of code.
- Tasks are **packaged into reusable modules** – these can either be installed from the public npm registry or developed locally within the project. As soon as a package is added, its tasks are immediately available to the project.
- Skivvy is **a task runner, not a build tool**. You can write your tasks however you want, Skivvy just provides a means of launching them instantly from the command-line. This means you're free to use any combination of [Gulp](http://gulpjs.com/)/[Broccoli](https://github.com/broccolijs/broccoli)/[Yo](https://github.com/yeoman/yo)/etc, all in the same project.


## How it works

There are 3 steps in the Skivvy workflow:

1. Initialize a project: `skivvy init`
2. Install packages: `skivvy install [package]`
3. Run tasks from the installed packages: `skivvy [task]`

The`skivvy` CLI tool automatically detects tasks from the packages that have been installed within the current project, as well as automatically loading any project-specific configuration for those tasks. This gives you instant access to your project's tasks, which are abstracted into reusable modules that can be shared across many different projects.

In practice, this allows you to take a [Docker](https://www.docker.com/)-like approach to creating a build system, where all the inner workings of the helper modules are completely isolated from the main application. This allows you to spend less time worrying about tooling, and more time coding.


## Usage instructions: Hello World app

1. Initialize a Skivvy project in a new folder:

    ```bash
    mkdir example-app && cd example-app
    skivvy init
    ```

2. Add the `hello` package to the current project:

    ```bash
    skivvy install hello
    ```

3. List the tasks installed by the `hello` package:

    ```bash
    skivvy list
    ```

    > _This will output something like the following:_
    ```bash
    example-app@0.0.1
    └─┬ hello@1.0.0
      └── start - Greet the user
    ```
    _This means that within our project we are now able to use the `hello:start` task, as it was included in the `hello` package._

4. Run the `hello:start` task:

    ```bash
    skivvy hello:start # Outputs: "Hello, world!"
    ```
    > _N.B. In the example above, the `hello:` package prefix is optional – `skivvy start` would also work._


## Available tasks

- `skivvy init` create a Skivvy project in the current directory
- `skivvy install [package]` Add a package to the current project
- `skivvy uninstall [package]` Remove a package from the current project
- `skivvy list [package]` List this project's installed packages and tasks
- `skivvy [task]` Run a task within the current project

All additional functionality is provided by the packages themselves. See the list of [existing Skivvy packages](#) for some common pre-made build tasks.


## Adding packages to a Skivvy project

Skivvy packages contain **tasks**, which are invoked from the command-line using `skivvy [task]`. Multiple packages can be installed into a single project, extending the project's build system in a modular fashion.

Skivvy packages come in two flavors: **external packages**, which have been published to the npm registry, and **local packages**, which exist as source files within the current project.


### Adding an external package

Skivvy packages can be installed via npm using the `skivvy install` command, as follows:

```bash
skivvy install my-custom-package
```

> _This will install the npm module named `skivvy-my-custom-package` from the npm registry, and add it to the current project's `devDependencies` field in its `package.json` file._

After running this command, any tasks defined in `my-custom-package` will automatically be available to the `skivvy` command-line tool when it is run from within this project.


### Adding a local package

Project-specific packages can be placed in the project's local Skivvy packages directory (`skivvy` by default, see below for instructions on how to change this).

Local packages use exactly the same structure as external packages. If any packages are present within a project's local package folder, their tasks will be automatically loaded by the CLI tool in addition to any external packages.


## Running Skivvy tasks

To run a task, use the `skivvy [task]` command as follows:

```bash
skivvy my-custom-task
```

This will search the installed packages for a task named `my-custom-task`, and run that task if it finds a match.

If multiple tasks across different packages are registered with the same name, the task name can be prefixed with the package name followed by a colon to avoid naming collisions:

```bash
skivvy my-custom-package:my-custom-task
```

The top-level tasks `init`, `install`, `uninstall` and `list` always take priority over tasks defined with the same name in other packages. The only way to run a task named `init`, `install`, `uninstall` or `list` from another package is via this colon-prefixed syntax.


### Passing custom configuration to Skivvy tasks (TODO)


#### Setting project configuration via the `skivvy.json` file (TODO)


#### Passing configuration via command-line flags (TODO)


#### Passing configuration via environment variables (TODO)


## Writing custom Skivvy packages (TODO)

Under the hood, a Skivvy package exposes all its tasks as plain JavaScript functions. This means that you can use any combination of Gulp/Broccoli/etc within a Skivvy project, even mixing multiple build systems in the same Skivvy package.


### Skivvy package folder structure (TODO)


### Skivvy package manifest file (TODO)

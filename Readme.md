# Red engine

This project is the [node-red](https://github.com/node-red) engine, consisting of the `api` and `runtime`, packaged as a [lerna](https://lernajs.io) project.

The project contains multiple related packages that can be managed as one project,
but where each package can be versioned, managed and published as a separate unit.

See [Lerna Getting Started](https://lernajs.io/#getting-started) for a typical development workflow.

## Quick start

Install `lerna` and `mocha` as a global binaries

```bash
npm i -g lerna mocha
```

## Prepare packages

In case the packages are not included in this container project, you can clone each of them into packages (alternatively fork each, then clone from your fork)

For *red-runtime*

```bash
red-engine/packages/red-runtime $ git clone git@github.com:tecla5/red-runtime.git
```

For *red-api*

```bash
red-engine/packages/red-api $ git clone git@github.com:tecla5/red-api.git
```

## Install package dependencies

Install dependencies for *red-api* (incl. local linking to modules in `/packages`)

```bash
cd packages/red-api
npm run lerna:update
```

Install dependencies for *red-runtime* (incl. local linking to modules in `/packages`)

```bash
cd packages/red-runtime
npm run lerna:update
```

## Run test suites via mocha

Run test suite for *red-api*

```bash
cd packages/red-api
mocha test/api/comms_spec.js
```

Run test suite for *red-runtime*

```bash
cd packages/red-runtime
mocha test/runtime/i18n_spec.js
```

See the current status of tests in `Testing.md` for *red-runtime* package.
No `Testing.md` document yet for *red-api* (make one!?)

## Lerna dependencies

A lerna package can been configured with internal (local) dependencies such as demonstrated in the `red-api` package:

```txt
  "dependencies": {
    ...
    "red-runtime": "x", // resolved locally to the lerna project if available
    ...
  }
```

Lerna will `npm link` to matching local packages in `red-engine/packages` if available. If not found locally, it will resolve the package dependency normally via npm registry, github or some other remote location.

This makes it much easier to do development on multiple inter-dependent packages simultaneously.

### Lerna quick update (relink all dependencies)

To make lerna easier to use, each package comes with a `lerna:update` script which updates all dependencies for a given package via lerna.

From the root folder of any package, run: `npm run lerna:update`

Example package update:

```bash
red-ui/packages/red-api $ npm run lerna:update
# lerna info ...
```

This will update and resolve all dependencies via lerna. Dependencies linked locally are linked via symbolic link as if the files are present inside the host project itself.

The lerna scripts are:

- `lerna:clean` - scrap all node_modules
- `lerna:bootstrap` - bootstrap node_modules
- `lerna:update` clean, then bootstrap

All the scripts are scoped to the particular package. If you run a lerna command without `--scope`, it will be run on all packages.

See [--scope option](https://github.com/lerna/lerna#--scope-glob) in [Lerna Readme](https://github.com/lerna/lerna#readme))

## Packages

This [lerna](https://lernajs.io/) project contains the following packages:

- `red-api` node-red API
- `red-runtime` node-red runtime

The packages are inter-dependent. The api contains one or more references to the runtime and the runtime one or more refs to the api.

### Red API

See [red-api](https://github.com/tecla5/red-runtime) repo for details

### Red Runtime

See [red-runtime](https://github.com/tecla5/red-runtime) repo for details

## Assets

Additionally the `/assets` folder contains the original node-red assets, including stylesheets, mustache templates etc.

## Community/FAQ

Ask questions on [node-red Slack channel](node-red.slack.com)

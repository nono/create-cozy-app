<h1 align="center">Cozy Scripts</h1>

<div align="center">
  <a href="https://www.npmjs.com/package/cozy-scripts">
    <img src="https://img.shields.io/npm/v/cozy-scripts.svg" alt="npm version" />
  </a>
  <a href="https://github.com/CPatchane/create-cozy-app/blob/master/packages/cozy-scripts/LICENSE">
    <img src="https://img.shields.io/npm/l/cozy-scripts.svg" alt="license" />
  </a>
  <a href="https://travis-ci.org/CPatchane/create-cozy-app">
    <img src="https://img.shields.io/travis/CPatchane/create-cozy-app.svg" alt="travis" />
  </a>
  <a href="https://npmcharts.com/compare/cozy-scripts">
    <img src="https://img.shields.io/npm/dm/cozy-scripts.svg" alt="npm downloads" />
  </a>
  <a href="https://david-dm.org/cpatchane/create-cozy-app?path=packages/cozy-scripts">
    <img src="https://david-dm.org/cpatchane/create-cozy-app/status.svg?path=packages/cozy-scripts" alt="david-dm" />
  </a>
  <a href="https://renovateapp.com/">
    <img src="https://img.shields.io/badge/renovate-enabled-brightgreen.svg" alt="renovate" />
  </a>
  <a href="https://github.com/facebook/jest">
    <img src="https://facebook.github.io/jest/img/jest-badge.svg" alt="tested with jest" />
  </a>
</div>

</br>

### What's cozy-scripts?

`cozy-scripts` is a script bundle partially designed to be run by `create-cozy-app`. This latter will just create the root folder and then run the `node_modules/cozy-scripts/scripts/init.js` script inside it with some CLI options.

All the application template outline is then handled by `cozy-scripts`.

To install:

```
yarn add --dev cozy-scripts
```

> If your purpose is to create an new application from scratch, don't install `cozy-scripts` but see [`create-cozy-app`](https://github.com/CPatchane/create-cozy-app/tree/master/packages/create-cozy-app) instead.

### Default template (`template` folder)

Using the default template, `cozy-scripts` will generate a React application. After the initialisation, you should have the following folder structure:

<details>

```
mycozyapp/
    babel.config.js
    CONTRIBUTING.md
    LICENSE
    README.md
    jest.config.js
    manifest.webapp
    node_modules/
    package.json
    yarn.lock
    src/
        assets/
        components/
            App.jsx
            Sidebar.jsx
            HelloViews/
            Todos/
        connections/allTodos.js
        locales/en.json
        styles/
        targets/
            browser/
                index.ejs
                index.jsx
            intents/
            mobile/
            vendor/
    test/
    .editorconfig
    .eslintrc.json
    .github/
        .ISSUE_TEMPLATE
        .PULL_REQUEST_TEMPLATE
    .gitignore
    .stylintrc
    .travis.yml
    .tx/
        config
```

</details>

This application is more than a simple Hello World, it's a mini Todo application doing interactions and data handling (using our new [`cozy-client`](https://github.com/cozy/cozy-client)) with the stack. A good way to understand how the application works with the Cozy.

### VueJS 2+ template (`template-vue` folder)

Using the VueJS template, `cozy-scripts` will generate a VueJS/Vuex application. After the initialisation, you should have the following folder structure:

<details>

```
mycozyapp/
    babel.config.js
    CONTRIBUTING.md
    LICENSE
    README.md
    jest.config.js
    manifest.webapp
    node_modules/
    package.json
    yarn.lock
    src/
        assets/
        components/
            App.vue
            Icon.vue
            HelloViews/
        lib/
            store.js
            I18n/
        locales/en.json
        styles/
        targets/
            browser/
                index.ejs
                index.js
            intents/
            mobile/
            vendor/
    test/
    .editorconfig
    .eslintrc.json
    .github/
        .ISSUE_TEMPLATE
        .PULL_REQUEST_TEMPLATE
    .gitignore
    .stylintrc
    .travis.yml
    .tx/
        config
```

</details>

### Ready to go

The script will download some dependencies (may take a while) and ask you a few questions, then create an application skeleton inside `mycozyapp`.

That's all! You can start hacking:

```
cd mycozyapp
yarn start
```

After the webpack build and the docker environment ready, your app should be available at http://mycozyapp.cozy.tools:8080

### The `cozy-scripts` CLI

`cozy-scripts` make available some scripts to be used inside your application (used by default in applications created from `create-cozy-app`):

##### - `cozy-scripts --show-config`

This command will output the webpack config computed according your current global variable `NODE_ENV`. By default, the application will have some `yarn` scripts (in the `package.json`) to show the configurations according to different environments (dev, browser, mobile, prod...).

##### - `cozy-scripts build`

This command will run webpack in a one shot execution of webpack (without files watching mode so) in a quiter way. It's recommended for production build.
The built files (destined to the Cozy) will be in `build/`.

> A `--debug` options is available if you want to ouput more informations about webpack building in your console.

##### - `cozy-scripts test`

Run all the application tests using internally [Jest](https://facebook.github.io/jest/). This command handles all parameters than Jest does, like `--watch` for the watch mode or `-u` to update snapshots for example.

##### - `cozy-scripts watch`

Unlike the previous command, this will run webpack using the files watching mode. Each time you will modify a file, a new build will triggered. It's recommended for development build.
The built files (destined to the Cozy) will be in `build/`.

> A `--debug` options is available if you want to ouput more informations about webpack building in your console.

##### - `cozy-scripts start`

This is the main way to launch your application for development.
This command will run webpack in a watching mode with a server (`webpack-dev-server`) to serve application assets.
Then, it will launch a Cozy stack using Docker (the image `cozy/cozy-app-dev`) to serve your application inside it.

Your application will be available at http://<MY_APP_SLUG>.cozy.tools:8080.

> In this mode [HMR (Hot Module Replacement)](https://webpack.js.org/concepts/hot-module-replacement/) is available to help you with the application development.

##### - `cozy-scripts publish`

This command will internally fetch and run the latest version of the [`cozy-app-publish` CLI](https://github.com/cozy/cozy-libs/tree/master/packages/cozy-app-publish) to publish your application on a Cozy registry (by default the official and main Cozy Cloud applications registry on `https://apps-registry.cozycloud.cc`). The options and arguments are the same than in the [`cozy-app-publish` package documentation](https://github.com/cozy/cozy-libs/tree/master/packages/cozy-app-publish).

##### - `cozy-scripts release`

This command will run a script to release a new version of the application.
The first step is to start the release using `cozy-scripts release start`. It will make a new release branch according to your current version and it will bump the version on your master branch.
Then you can release new versions (on your release branch) using `cozy-scripts release patch|beta|stable` according to your needs.

By default, this script will push on your git `origin` remote but you can change by passing it to your script after the action name: `cozy-scripts release start cozy` to use the remote named `cozy`.

__You can find more informations about this library and how to use it in [`cozy-release` documentation](https://github.com/cozy/cozy-libs/tree/master/packages/cozy-app-publish)__

> :warning: __BE VERY CAREFUL__ using this script since __it will push directly to your remote repository__. A prompt will warn you before starting the release.

##### - `--vue`

Allow to use the VueJS webpack configuration as default opinionated configuration. You don't need an `app.config.js` to use the VueJS webpack bundle configuration (`wepack.bundle.vue.js`) with this option.

##### - `--production` or `--development` options

Allow to pass the wanted build mode to `cozy-scripts`. This mode will be overwritten by `process.env.NODE_ENV` usage (ex: `browser:development` for development usage with browser target).

##### - `--browser` or `--mobile` options

Allow to pass the wanted build target to `cozy-scripts`. This target will be overwritten by `process.env.NODE_ENV` usage (ex: `browser:development` for development usage with browser target).

##### - `--analyzer`

Use this option if you want to analyze your builds content using the webpack plugin [`webpack-bundle-analyzer`](https://github.com/webpack-contrib/webpack-bundle-analyzer). It will open you browser with an interactive treemap visualization of the contents of all your bundles.

##### - `--no-stack`

Use this option if you want to run `cozy-scripts start` without launching the Cozy stack using docker. You will only have Webpack watching with `webpack-dev-server`.
It could be useful if you already have a Cozy stack running elsewhere in your environment.

##### - Custom paths providing options: `--src-dir`, `--build-dir`, `--manifest`

Use these options if you want to `build`/`watch`/`start` your application with custom paths. These paths __must be relative to the application root directory__:

- `--src-dir`: the `src` directory, the source files of your application
- `--build-dir`: the directory to put the application build files into
- `--manifest`: the path of your manifest file `manifest.webapp` (the `.webapp` extension must be provided)

### The `cozy-scripts` webpack configuration

`cozy-scripts` is designed to use a default webpack configuration for a basic React/Redux (or VueJS) application which uses `cozy-ui` and `cozy-client-js`. But you can override or use your custom configuration files by creating a new `app.config.js` file in your application root folder. Here is an example to overload the default bundle config with a custom one:

```javascript
// myapp/app.config.js
module.exports = [
  // default for React/Redux
  require('cozy-scripts/config/webpack.bundle.default.js'),
  // here is the equivalent one for vue
  // require('cozy-scripts/config/webpack.bundle.vue.js'),
  require('./config/webpack.myconfig.js')
]
```

You can find more information about webpack configuration files available via `cozy-scripts` in the dedicated [webpack configs documentation](docs/webpack-configs.md).

If you need more particular/complicated configurations and need to use the [`webpack-merge`](https://github.com/survivejs/webpack-merge#merging-with-strategies) smart mode or merge strategies, you can also find more information about in the dedicated [merge strategies documentation](docs/webpack-merge-strategies.md).

> :warning: `cozy-scripts` internally uses __Webpack v4__, be sure to use Webpack 4 compatible configurations if you want to provide some custom __configurations in the `app.config.js`__

### `cozy-scripts` & `cozy-flags`
`cozy-scripts` works well with [cozy-flags](https://www.npmjs.com/package/cozy-flags). You can specify a few flags on build time : 

```
COZY_FLAGS=flag1,flag2 yarn build
```

```js
import flag from 'cozy-flags'
 
if (flag('flag1') || flag('flag2')) {
  enableMyFeature()
}
```


## Community

### What's Cozy?

<div align="center">
  <a href="https://cozy.io">
    <img src="https://cdn.rawgit.com/cozy/cozy-site/master/src/images/cozy-logo-name-horizontal-blue.svg" alt="cozy" height="48" />
  </a>
 </div>
 </br>

[Cozy] is a platform that brings all your web services in the same private space.  With it, your webapps and your devices can share data easily, providing you with a new experience. You can install Cozy on your own hardware where no one's tracking you.

### Get in touch

You can reach the Cozy Community by:

- Chatting with us on IRC [#cozycloud on Freenode][freenode]
- Posting on our [Forum][forum]
- Posting issues on the [Github repos][github]
- Say Hi! on [Twitter][twitter]


## License

`cozy-scripts` is distributed under the MIT license.

### Credits

A big thanks to [@CPatchane](https://github.com/CPatchane), the original author of Create-Cozy-App (CCA) tools.


[cozy]: https://cozy.io "Cozy Cloud"
[freenode]: http://webchat.freenode.net/?randomnick=1&channels=%23cozycloud&uio=d4
[forum]: https://forum.cozy.io/
[github]: https://github.com/cozy/
[twitter]: https://twitter.com/cozycloud

# Don't reinvent the wheel

Before writing a new plugin, first check the [Plugin Registry](https://github.com/haraka/Haraka/blob/master/Plugins.md) to see if that plugin exists.

# Nomenclature

Use the `haraka-plugin-[UNIQUE]` naming style. We're moving towards npm packaging most plugins and the `haraka-plugin` prefix is the nomenclature.

## package plugin for release on npm.

Clone the [haraka-plugin-template](https://github.com/haraka/haraka-plugin-template).

Or set up the plugin directory yourself:

```sh
mkdir haraka-plugin-example && cd haraka-plugin-example
touch index.js       // plugin code goes here
mkdir config test
echo '[main]' > config/example.ini
echo "const fixtures = require('haraka-test-fixtures');" > test/main.js
echo '# haraka-plugin-example' > README.md
echo "# 1.0.0 - $(date +%Y-%m-%d)" > Changes.md
npm init
npm install --save <npm modules your plugin requires>
npm install --save-dev haraka-test-fixtures eslint eslint-plugin-haraka
```

## config loading

Use this standard method for config loading:

```js
exports.register = function () {
    var plugin = this;
    plugin.load_example_ini();
}
exports.load_example_ini = function () {
    var plugin = this;
    plugin.cfg = plugin.config.get('example.ini', function () {
        plugin.load_example_ini();
    });
}
```

- the callback added to `config.get()` lets your plugins config get updated immediately when the config changes. No Haraka restart required.
- storing the config contents at plugin.cfg lets the active configuration be accessed and manipulated by external processes. This is used heavily when adding test coverage.

### booleans

If you have true/false booleans in your config, declare them and optionally, their defaults as well:

```js
exports.load_example_ini = function () {
    var plugin = this;
    plugin.cfg = plugin.config.get('example.ini', {
        booleans: [
            '+enabled',               // plugins.cfg.main.enabled=true
            '-disabled',              // plugins.cfg.main.disabled=false
            '+feature_section.yes'    // plugins.cfg.feature_section.yes=true
        ]
    },
    function () {
        plugin.load_example_ini();
    });
}
```

## enable Travis-CI testing

Add a .travis.yml config file to your repo:

```yaml
language: node_js
node_js:
  - "4"
  - "6"

services:
# https://docs.travis-ci.com/user/database-setup/
#  - mongodb
#  - elasticsearch
#  - redis-server

before_script:

script:
    - npm run lint
    - npm test

after_success:

sudo: false
```

The services section can start up DB instances to test against. Use your favorite testing framework to write some basic tests, like config loading, connecting to DBs and then validating that your functions do what you expect.

## Lint testing with eslint

We ask that you [at least mostly] use the coding style in haraka/recommended. We've made it easy by publishing an eslint plugin that imports our rules automatically. Add an `.eslintrc.json` file to your project dir:

```json
{
  "plugins": [
    "haraka"
  ],
  "extends": ["plugin:haraka/recommended"],
  "rules": {
    "no-console": 0
  }
}
```

and add these two lines in the `scripts` section of package.json:

```js
    "lint": "./node_modules/.bin/eslint *.js test/**/*.js",
    "lintfix": "./node_modules/.bin/eslint --fix *.js test/**/*.js",
```

Now you can lint test with `npm run lint` and correct any lint issues automatically with `npm run lintfix`. With Travis-CI enabled lint testing happens automatically (see the .travis.yml above) on every PR. This means that anyone submitting PRs against your plugin will have their changes linted automatically. It reduces errors and bugs and saves everyone time.

I also add `"eslint:recommended"` to the extends section and find it helpful. You may consider it too.

### codeclimate

If you want a codeclimate badge to display in your README, you'll need to specify eslint version 3 (they default to 2) in a .codeclimate.yml file. Otherwise the codeclimate import will fail:

```yaml
engines: 
  eslint:
    enabled: true
    channel: "eslint-3"
    config:
      config: ".eslintrc.json"

ratings:
   paths:
   - "**.js"
```

## Windows Compatibility

We do our Windows testing on [Appveyor](https://www.appveyor.com). It must be configured in an appveyor.yml file like this:

```yaml
environment:
  nodejs_version: "4"

install:
  - npm install

before_build:
build: off
after_build:

before_test:
  - node --version
  - npm --version

test_script:
  - node run_tests

after_test:
```
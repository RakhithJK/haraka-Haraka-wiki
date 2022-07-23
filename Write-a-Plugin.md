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
    this.load_example_ini();
}
exports.load_example_ini = function () {
    const plugin = this;
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
    const plugin = this;
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


## Lint testing with eslint

We ask that you [at least mostly] use the coding style in haraka/recommended. We've made it easy by publishing an eslint plugin that imports our rules automatically. Add an `.eslintrc.yaml` file to your project dir:

```yaml
env:
  node: true
  es6: true
  mocha: true

plugins:
  - haraka

extends:
  - eslint:recommended
  - plugin:haraka/recommended

rules:
  indent: [2, 2, {"SwitchCase": 1}]

root: true

globals:
  OK: true
  CONT: true
  DENY: true
  DENYSOFT: true
  DENYDISCONNECT: true
  DENYSOFTDISCONNECT: true
```

and add these two lines in the `scripts` section of package.json:

```js
    "lint": "npx eslint *.js test",
    "lintfix": "npx eslint --fix *.js *.js",
```

Now you can lint test with `npm run lint` and correct any lint issues automatically with `npm run lintfix`.

I also add `"eslint:recommended"` to the extends section and find it helpful. You may consider it too.

### codeclimate

If you want a codeclimate badge to display in your README, you'll need to specify eslint version 3 (they default to 2) in a .codeclimate.yml file. Otherwise the codeclimate import will fail:

```yaml
engines: 
  eslint:
    enabled: true
    channel: "eslint-8"
    config:
      config: ".eslintrc.yaml"

ratings:
   paths:
   - "**.js"
```

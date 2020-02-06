# lib-config

in your start up file you would have something like this

```js
const config = require('@luxuryescapes/lib-config')
config.load({
  env: 'local', // optional, defaults to process.env.APP_ENV,
  schema: {
    // json schema of the config schema
    type: 'object',
    properties: {
      port: { type: 'integer' },
      apiEndpoint: { type: 'string' }
    },
    required: ['port', 'apiEndpoint'],
    additionalProperties: false
  },
  configDir: 'myconfig' // optional, default to `config`
})
```

then in other files you can do the following

```js
const config = require('@luxuryescapes/lib-config')

express.listen(config.get().port)
```

get your test runner to load config as the first step

```js
process.env.APP_ENV = 'spec'
const config = require('@luxuryescapes/lib-config')
config.load()
```

or

```js
const config = require('@luxuryescapes/lib-config')
config.load({ env: 'spec' })
```

if you want to mock config for unit testing you can do something like this

```js
const sinon = require('sinon')
const config = require('@luxuryescapes/lib-config')

beforeEach(() => {
  sinon.sandbox(config, 'get').returns({
    ...config.get(),
    myFeatureEnabled: false
  })
})

afterEach(() => {
  sinon.sandbox.restore()
})
```

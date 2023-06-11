## Install vuejs

```bash
npm install -g @vue/cli  # Install vuejs globally
```

On the level of manage.py file we create a vuejs project
```bash
vue create frontend
```
```bash
cd frontend
npm run serve    # try run vuejs app
```

create 'vue.config.js' file inside frontend and same level with node module

```javascript
const BundleTracker = require('webpack-bundle-tracker');

module.exports = {

    publicPath: "http://0.0.0.0:8080",

    outputDir: "./dist/",

    chainWebpack: config => {
        config.optimization.splitChunks(false)
        config.plugin('BundleTracker').use(BundleTracker, [
            {
                filename: './webpack-stats.json'
            }
        ])

        config.resolve.alias.set('__STATIC__', 'static')

        config.devServer
            .public('http://0.0.0.0:8080')
            .host('0.0.0.0')
            .port(8080)
            .hotOnly(true)
            .watchOptions({poll: 1000})
            .https(false)
            .headers({'Access-Control-Allow-Origin': ['\*']})
    }

};
```
to connect django to vuejs app we need  webpack-bundle-tracker
```bash
npm install --save-dev webpack-bundle-tracker
```


## Config django
```bash
pip install django-webpack-loader
```

First of all, add `webpack_loader` to `INSTALLED_APPS`

```python
INSTALLED_APPS = (
  ...
  'webpack_loader',
  ...
)
```

Below is the recommended setup for the Django settings file when using `django-webpack-loader`.
```python
WEBPACK_LOADER = {
  'DEFAULT': {
    'CACHE': not DEBUG,
    'STATS_FILE': os.path.join(BASE_DIR,'frontend', 'webpack-stats.json'),
    'POLL_INTERVAL': 0.1,
    'IGNORE': [r'.+\.hot-update.js', r'.+\.map'],
  }
}
```

## Rendering

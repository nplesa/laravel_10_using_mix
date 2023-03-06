# laravel_10_using_mix
Settings which must be done for using mix with Laravel 10

1. Install Laravel Mix
First, you will need to install Laravel Mix using your npm package manager of choice:

npm install --save-dev laravel-mix

2. Configure Mix
Create a webpack.mix.js file (touch webpack.min.js) in the root of your project and add this content:

const mix = require('laravel-mix');
```
/*
 |--------------------------------------------------------------------------
 | Mix Asset Management
 |--------------------------------------------------------------------------
 |
 | Mix provides a clean, fluent API for defining some Webpack build steps
 | for your Laravel applications. By default, we are compiling the CSS
 | file for the application as well as bundling up all the JS files.
 |
 */

mix.js('resources/js/app.js', 'public/js')
    .postCss('resources/css/app.css', 'public/css', [
        //
    ]);
```
3. Update NPM scripts
Update your NPM scripts in package.json:
```
  "scripts": {
// //remove these 2 lines  
-     "dev": "vite",
-     "build": "vite build"

// add next lines 
     "dev": "npm run development",
     "development": "mix",
     "watch": "mix watch",
     "watch-poll": "mix watch -- --watch-options-poll=1000",
     "hot": "mix watch --hot",
     "prod": "npm run production",
     "production": "mix --production"
  }
```
4. In .env file
```
- remove lines:
VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

-and add:
MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_HOST="${PUSHER_HOST}"
MIX_PUSHER_PORT="${PUSHER_PORT}"
MIX_PUSHER_SCHEME="${PUSHER_SCHEME}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
```
5. In file resources/js/bootstrap.js, you must have just these lines:
```

window._ = require('lodash');

/**
 * We'll load jQuery and the Bootstrap jQuery plugin which provides support
 * for JavaScript based Bootstrap features such as modals and tabs. This
 * code may be modified to fit the specific needs of your application.
 */

try {
    window.$ = window.jQuery = require('jquery');
    window.Popper = require('@popperjs/core');
    window.bootstrap = require('bootstrap');

} catch (e) {}

/**
 * We'll load the axios HTTP library which allows us to easily issue requests
 * to our Laravel back-end. This library automatically handles sending the
 * CSRF token as a header based on the value of the "XSRF" token cookie.
 */

window.axios = require('axios');

window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
```
6. Remove Vite and the Laravel Vite Plugin from npm
npm remove vite laravel-vite-plugin

7. Remove your Vite configuration file:
rm vite.config.js

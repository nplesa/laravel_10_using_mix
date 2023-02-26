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
/**
 * We'll load the axios HTTP library which allows us to easily issue requests
 * to our Laravel back-end. This library automatically handles sending the
 * CSRF token as a header based on the value of the "XSRF" token cookie.
 */

import axios from 'axios';
window.axios = axios;

window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';

/**
 * Echo exposes an expressive API for subscribing to channels and listening
 * for events that are broadcast by Laravel. Echo and event broadcasting
 * allows your team to easily build robust real-time web applications.
 */

// import Echo from 'laravel-echo';

// import Pusher from 'pusher-js';
// window.Pusher = Pusher;

// MIX
// window.Echo = new Echo({
//     broadcaster: 'pusher',
//     key: process.env.MIX_PUSHER_APP_KEY,
//     wsHost: process.env.MIX_PUSHER_HOST ?? `ws-${process.env.MIX_PUSHER_APP_CLUSTER}.pusher.com`,
//     wsPort: process.env.MIX_PUSHER_PORT ?? 80,
//     wssPort: process.env.MIX_PUSHER_PORT ?? 443,
//     forceTLS: (process.env.MIX_PUSHER_SCHEME ?? 'https') === 'https',
//     enabledTransports: ['ws', 'wss'],
// });

6. Remove Vite and the Laravel Vite Plugin from npm
npm remove vite laravel-vite-plugin

7. Remove your Vite configuration file:
rm vite.config.js

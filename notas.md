# 3 Formas trabajar con PWA
## con CRA
- Obtengo configuraciones de worbox con CRA
```
npx create-react-app my-app --template cra-template-pwa
- Agrego archivos `service-worker.js` y `serviceWorkerRegistration.js`
```
- Agrego dependencias
```
"workbox-background-sync": "^6.5.4",
    "workbox-broadcast-update": "^6.5.4",
    "workbox-cacheable-response": "^6.5.4",
    "workbox-core": "^6.5.4",
    "workbox-expiration": "^6.5.4",
    "workbox-google-analytics": "^6.5.4",
    "workbox-navigation-preload": "^6.5.4",
    "workbox-precaching": "^6.5.4",
    "workbox-range-requests": "^6.5.4",
    "workbox-routing": "^6.5.4",
    "workbox-strategies": "^6.5.4",
    "workbox-streams": "^6.5.4"
```
- en `index.js`
```
import * as serviceWorkerRegistration from './serviceWorkerRegistration';

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://cra.link/PWA
serviceWorkerRegistration.register();
```
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

## Metodo Manual
```
self.addEventListener('install', (event) => {
  console.log('Service Worker installing.');
})
```
- Guardar en cache
```
self.addEventListener("install", async (event) => {
  const cache = await caches.open("v1");

  await cache.addAll([
    "https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css",
    "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.12.0-2/css/all.min.css",
    "/favicon.ico",
  ]);
});
```
- Interceptar peticiones
```
self.addEventListener("fetch", (event) => {

  console.log("fetch event", event.request.url);
})
```
### Network first with cache fallback
- Ejemplo vert si jwt es valido para mantenerlo en la pantalla
```
self.addEventListener("fetch", (event) => {
  //console.log("fetch event", event.request.url);
  if (event.request.url !== "http://localhost:4000/api/auth/renew") return;

  const resp = fetch(event.request).then((response) => {
    //Guardar en cache la respuesta
    caches.open("cache-dynamic").then((cache) => {
      cache.put(event.request, response);
    })
    return response.clone();
  }).catch((err) => {
    console.log("offline response");
    return caches.match(event.request);
  })

  event.respondWith(resp);
});
```

## Workbox CLI
- Service Worker solo se puede instalar en el build de produccion
- en `index.html`
```
<script>
      const isProduction = "%NODE_ENV%" === "production";

      if (isProduction && "serviceWorker" in navigator) {
        navigator.serviceWorker.register("/sw.js");
      }
</script>
```
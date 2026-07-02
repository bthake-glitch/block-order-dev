const CACHE = 'bt-block-order-dev-v5-22-pdf-pics';
const FILES = ["./", "./index.html", "./manifest.webmanifest", "./icon-192.png", "./icon-512.png", "./icon-yellow-192.png", "./icon-yellow-512.png", "./block-img/10_01.png", "./block-img/10_31.png", "./block-img/15_01.png", "./block-img/15_02.png", "./block-img/15_03.png", "./block-img/15_12.png", "./block-img/15_20.png", "./block-img/15_22.png", "./block-img/15_42.png", "./block-img/15_45.png", "./block-img/15_48.png", "./block-img/15_71.png", "./block-img/15_801.png", "./block-img/20_01.png", "./block-img/20_011.png", "./block-img/20_02.png", "./block-img/20_03.png", "./block-img/20_09.png", "./block-img/20_10.png", "./block-img/20_12.png", "./block-img/20_13.png", "./block-img/20_18.png", "./block-img/20_20.png", "./block-img/20_21.png", "./block-img/20_22.png", "./block-img/20_25.png", "./block-img/20_42.png", "./block-img/20_45.png", "./block-img/20_45A_15.png", "./block-img/20_45A_20.png", "./block-img/20_45A_30.png", "./block-img/20_48.png", "./block-img/20_61.png", "./block-img/20_71.png", "./block-img/20_739.png", "./block-img/20_925.png", "./block-img/30_02.png", "./block-img/30_03.png", "./block-img/30_18.png", "./block-img/30_45.png", "./block-img/30_48.png", "./block-img/30_925.png", "./block-img/40_925.png", "./block-img/50_31c.png"];

self.addEventListener('install', e => {
  self.skipWaiting();
  e.waitUntil(caches.open(CACHE).then(c => c.addAll(FILES)));
});

self.addEventListener('activate', e => {
  e.waitUntil(
    caches.keys()
      .then(keys => Promise.all(keys.filter(k => k !== CACHE).map(k => caches.delete(k))))
      .then(() => self.clients.claim())
  );
});

self.addEventListener('fetch', e => {
  const req = e.request;
  if(req.mode === 'navigate'){
    e.respondWith(fetch(req).catch(() => caches.match('./index.html')));
    return;
  }
  e.respondWith(caches.match(req).then(r => r || fetch(req)));
});

## serviceWorker缓存实现

1. 创建serviceWorker实例

    	window.addEventListener("load",()=>{
			if("serviceWorker" in navigator){
				navigator.serviceWorker.register("./sw.js",{
				scope:"./"
			}).then(res=>{
				console.log(res.scope);
			}).catch(err=>{
				console.log(err);
			})
			}
		})


2. 在sw.js文件家里面设置

>添加缓存

    let cacheName='my_me'
    self.addEventListener("install",event=>{
    caches.open(cacheName).then(cache=>cache.addAll([
        "/index.html",
        "/img/01.jpg",
        "/manifest.json",
        "/css/01.css",
        "/css/02.css"
    ]))
    self.skipWaiting()
    })


>在没有网络的情况下判断资源

    self.addEventListener('fetch', function(event) {
    event.respondWith(
      caches.match(event.request)
      .then(function(response) {
        if (response) {
          return response;
        }
        return fetch(event.request);
      })
    )});


3. 软件设置

>manifest.json


  {
    "name": "HackerWeb",
    "short_name": "HackerWeb",
    "start_url": "index.html",
    "display": "standalone",
    "background_color": "#fff",
    "description": "A simply readable Hacker News app.",
    "icons": [ {
      "src": "./img/01.jpg",
      "sizes": "168x168",
      "type": "img/jpg"
    }]
}


# 使用 railway 部署 Vmess Vless + WS 协议传输，并配置 Web 网站。每次部署自动选择最新的 Alpine Linux 和 X-core 。

> 提醒： 滥用可能导致账户被BAN！！！

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://railway.app)
## X 默认配置
  ```bash
    * 协议：Vmess或Vless
    * 地址：自选 IP
    * 端口：443
    * 默认UUID：5aaed9b7-7fe3-47c3-bb52-db59859ce198
    * 加密：none
    * 传输协议：ws
    * 伪装类型：none
    * 伪装域名：xxx.workers.dev (Cloudflare Workers 反代地址)
    * 路径：/api-vmess或vless
    * 底层传输安全：tls
    * 跳过证书验证：false
    * SNI：xxx.workers.dev (Cloudflare Workers 反代地址)


## 流量中转

  <summary>可以使用 Cloudflare 的 Workers 来中转流量，配置为：</summary>
  
  ```js
  const SingleDay = 'xxx.up.railway.app';
  const DoubleDay = 'xxx.up.railway.app';
  addEventListener(
      "fetch", event => {
          let nd = new Date();
          if (nd.getDate() % 2) {
              host = SingleDay;
          } else {
              host = DoubleDay;
          }
          let url = new URL(event.request.url);
          url.hostname = host;
          let request = new Request(url, event.request);
          event.respondWith(
              fetch(request)
          )
      }
  )
  ```


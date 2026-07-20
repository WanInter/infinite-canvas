# SERVER_INFO.md

## 生产部署

- 主机：`ali`（`8.141.2.80:10022`）
- 应用目录：`/srv/infinite-canvas`
- 域名：`canvas.waninter.com`
- 镜像：`ghcr.io/waninter/infinite-canvas:latest`
- 运行方式：Docker Compose；应用仅绑定 `127.0.0.1:3000`，由宿主机 Caddy 反向代理并处理 HTTPS。

## 更新

```bash
ssh ali
cd /srv/infinite-canvas
docker compose pull
docker compose up -d
```

## Caddy

宿主机配置文件为 `/etc/caddy/Caddyfile`，站点配置如下：

```caddy
canvas.waninter.com {
    reverse_proxy 127.0.0.1:3000
}
```

修改后执行 `caddy validate --config /etc/caddy/Caddyfile`，再执行 `systemctl reload caddy`。

## DNS

`canvas.waninter.com` 需要将 A 记录指向 `8.141.2.80`，Caddy 才能签发并续期 HTTPS 证书。

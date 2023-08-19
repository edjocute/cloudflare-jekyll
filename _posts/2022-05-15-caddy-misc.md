---
title: "Caddy notes"
date: 2022-05-15T13:30:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - VPS
toc: true
usemathjax: false
copybutton: true
---

## 1. Vaultwarden

If using Cloudflare proxy, we need to set `X-Real-IP` header to that of the remote host (not Cloudflare):
```
@vw host bitwarden.domain.com
handle @vw {
  handle_path /notifications/hub/negotiate/ {
	 reverse_proxy localhost:9000
	 header_up X-Real-IP {http.request.header.CF-Connecting-IP}
  }

  handle_path /notifications/hub/ {
	 reverse_proxy localhost:3012
	 header_up X-Real-IP {http.request.header.CF-Connecting-IP}
  }

  reverse_proxy localhost:9000 {
	 header_up X-Real-IP {http.request.header.CF-Connecting-IP}
  }
}
```

This also appears to let websockets work properly as well (no errors from `vaultwarden::api::notifications`.


## 2. Rewriting headers for Cloudflare ([ref](https://www.reddit.com/r/selfhosted/comments/sixkst/fail2ban_with_cloudflare_proxy/))

```
reverse_proxy localhost:9999 {

header_up X-Real-IP {http.request.header.CF-Connecting-IP}
header_up X-Forwarded-For {http.request.header.CF-Connecting-IP}
header_up X-Forwarded-Host {http.request.hostport}
}
```




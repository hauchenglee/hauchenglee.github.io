---
layout: post
title: Docker Visualization
category: it
tags: [devops]
---

圖形化界面管理工具：
- portainer（先用這個）
- rancher（CI/CD在用）

```shell
docker run -d -p 8088:9000 \
-restart-alaways -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

---
# dept-cloud-addr

云端业务服务器地址公告牌（地址发布仓库）。

## 用途

集中发布云端业务系统各隧道的实时公网地址，供客户端多地址自动切换。

## 文件说明

- `addr.txt` — 地址公告（多行格式）：
  ```
  biz=https://xxx.trycloudflare.com
  backup1=https://xxx.trycloudflare.com
  backup2=https://xxx.trycloudflare.com
  backup3=https://xxx.trycloudflare.com
  backup4=https://xxx.trycloudflare.com
  backup5=https://xxx.trycloudflare.com
  backup6=https://xxx.trycloudflare.com
  ```
- `index.html` — 网页展示页（便于浏览器直接查看公告）。
- `status.json` — 隧道健康状态汇总（由 addr-health workflow 每 10 分钟生成）。

## 更新机制

由 `dept-cloud-keepalive` 仓库的 GitHub Actions workflow 在隧道重建成功后
自动写入对应行（主隧道写 `biz`，备用隧道写 `backupN`），无需人工干预。

## 访问方式

- GitHub Pages：`https://1759717772.github.io/dept-cloud-addr/addr.txt`
- Raw：`https://raw.githubusercontent.com/1759717772/dept-cloud-addr/main/addr.txt`
- CDN 镜像：`https://cdn.jsdelivr.net/gh/1759717772/dept-cloud-addr@main/addr.txt`

## 维护注意事项

- 各 workflow 只写自己的行（biz / backupN），并发写入由 board.sh 的
  fetch+重试逻辑保证不覆盖其他行。
- 健康监控 workflow（addr-health.yml）会检查全部隧道（biz/biz2/backup1-6）。
